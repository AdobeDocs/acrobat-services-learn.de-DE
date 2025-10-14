---
title: Erstellen und Bearbeiten von Berichten
description: Erfahren Sie, wie Sie PDF-Berichte für Kunden auf Ihrer Website generieren.
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8093
thumbnail: KT-8093.jpg
exl-id: 2f2bf1c2-1b33-4eee-9fd2-5d0b77e6b0a9
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1292'
ht-degree: 0%

---

# Erstellen und Bearbeiten von Berichten

![Banner &quot;Use Case Hero&quot;](assets/UseCaseReportHero.jpg)

In Finanz-, Bildungs-, Marketing- und anderen Branchen nutzen PDF den Austausch von Daten mit Kunden und Stakeholdern. PDF erleichtern den Austausch umfangreicher Dokumente mit Tabellen, Grafiken und interaktiven Inhalten in einem für jeden sichtbaren Format. [!DNL Adobe Acrobat Services]-APIs helfen diesen Unternehmen, gemeinsam nutzbare PDF-Berichte aus Microsoft Word, Microsoft Excel, Grafiken und anderen diversen Dokumentformaten zu erstellen.

Angenommen, Sie [führen ein Social-Media-Tracking-Unternehmen aus](https://developer.adobe.com/document-services/use-cases/content-publishing/on-demand-report-creation). Ihre Kunden melden sich bei einem passwortgeschützten Teil Ihrer Website an, um ihre Kampagnenanalysen anzuzeigen. Oft möchten sie diese Statistiken mit ihren Führungskräften, Aktionären, Spendern oder anderen Stakeholdern teilen. Herunterladbare PDF-Dokumente sind eine großartige Möglichkeit für Ihre Kunden, Zahlen, Diagramme und mehr zu teilen.

Durch die Integration von [PDF Services API](https://developer.adobe.com/document-services/apis/pdf-services) in Ihre Website können Sie unterwegs für jeden Kunden PDF-Berichte erstellen. Ihr könnt PDF erstellen und diese anschließend zu einem einzigen, praktischen Bericht zusammenfassen, den eure Kunden herunterladen und an ihre Stakeholder weitergeben können.

## Lernziel.

In diesem Tutorial lernen Sie, wie Sie das PDF Services SDK in einer Node.js- und einer Express.js-Umgebung (mit nur einigen JavaScript-, HTML- und CSS-Dateien) verwenden, um einer bestehenden Website schnell und einfach PDF-orientierte Funktionen hinzuzufügen. Diese Website umfasst eine Seite, auf der Administratoren Berichte hochladen, einen Bereich, in dem Kunden eine Liste der verfügbaren Berichte anzeigen und Dokumente auswählen, die in den PDF konvertiert werden sollen, sowie hilfreiche Endpunkte, um vom System generierte PDF herunterzuladen.

## Relevante APIs und Ressourcen

* [PDF Services-API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF Embed-API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

## Dashboard für Kampagnenberichte für Kunden

>[!NOTE]
>
>In diesem Tutorial geht es nicht um die Best Practices von Node.js oder wie Sie Ihre Webanwendungen schützen können. Einige Bereiche der Website sind öffentlich zugänglich, und die Benennung von Dokumenten kann nicht produktionsfreundlich sein. Um den bestmöglichen Ansatz für den Entwurf eines solchen Systems zu besprechen, fragen Sie Ihre Architekten und Ingenieure.

Hier verfügen Sie über eine grundlegende Express.js-Webanwendung, die über einen Bereich für Kundenberichte und einen Administratorbereich verfügt. Diese Anwendung kann Berichte für Social-Media-Kampagnen präsentieren. Beispielsweise kann veranschaulicht werden, wie oft auf eine Werbung geklickt wird.

![Screenshot des Erhalts personalisierter Berichte](assets/report_1.png)

Sie können dieses Projekt aus dem [GitHub-Repository](https://github.com/afzaal-ahmad-zeeshan/express-adobe-pdf-tools) herunterladen.

Sehen wir uns nun an, wie die Berichte veröffentlicht werden können.

## Hochladen von Berichten

Um es einfach zu halten, verwenden Sie hier nur das dateisystembasierte Hochladen und Verarbeiten. In Express.js können Sie das fs-Modul verwenden, um alle verfügbaren Dateien unter einem Verzeichnis aufzulisten.

Aktivieren Sie auf derselben Seite den Administrator, um Berichtsdateien auf den Server hochzuladen, damit Kunden sie sehen können. Diese Dateien können in vielen verschiedenen Formaten vorliegen, z. B. Microsoft Word, Microsoft Excel, HTML und [andere Datenformate](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf), einschließlich Grafikdateien. Die Verwaltungsseite sieht folgendermaßen aus:

![Screenshot der Admin-Funktion](assets/report_2.png)

>[!NOTE]
>
>Schützen Sie Ihre URLs mit einem Kennwort oder verwenden Sie das Passpaket von npm, um Ihre Anwendung hinter der Authentifizierungs- und Autorisierungsebene zu schützen.

Wenn der Administrator eine Datei auswählt und hochlädt, wird sie in ein öffentliches Repository verschoben, wo andere Personen darauf zugreifen können. Sie verwenden dasselbe Repository, um Dokumente von der Admin-Seite aus zu veröffentlichen und die verfügbaren Marketing-Berichte für Kunden aufzulisten. Dieser Code lautet:

```
router.get('/', (req, res) => {
try {
let files = fs.readdirSync('./public/documents/raw') // read the files
res.status(200).render("reports", { page: 'reports', files: files });
} catch (error) {
res.status(500).render("crash", { error: error });
}
});
```

Dieser Code listet alle Dateien auf und rendert eine Ansicht der Dateiliste.

## Auswählen von Berichten

Auf der Nutzerseite hast du ein Formular, über das Kunden Dokumente auswählen können, die sie in ihren Kampagnenbericht für Social Media aufnehmen möchten. Der Einfachheit halber sollten Sie auf der Beispielseite nur den Dokumentnamen und ein Kontrollkästchen zur Auswahl des Dokuments anzeigen. Kunden können einen oder mehrere Berichte zum Zusammenführen in einem einzigen PDF-Dokument auswählen.

Für eine erweiterte Benutzeroberfläche können Sie hier auch eine Vorschau des Berichts anzeigen.

![Screenshot der Kundenfunktionalität](assets/report_3.png)

## Generieren eines PDF-Berichts

Verwenden Sie das PDF Services SDK, um die PDF-Berichte aus Ihren Dateneingaben zu erstellen. Die Daten (wie in den Screenshots oben gezeigt) können aus verschiedenen Datenformaten wie Microsoft Word, Microsoft Excel, HTML, Grafiken und mehr stammen. Beginnen Sie mit der Installation des npm-Pakets für PDF Services SDK.

```
$ npm install --save @adobe/documentservices-pdftools-node-sdk
```

Bevor Sie beginnen, müssen Sie über API-Zugangsberechtigungen verfügen, [, die auf dem Adobe &#x200B;](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred) kostenlos sind. Verwenden Sie Ihr [!DNL Acrobat Services]-Konto [&#x200B; sechs Monate lang kostenlos, und bezahlen Sie dann nach Bedarf](https://developer.adobe.com/document-services/pricing/main) für nur \$0,05 pro Dokumenttransaktion.

Laden Sie die Archivdatei herunter und extrahieren Sie die JSON-Datei für Anmeldeinformationen und den privaten Schlüssel. Im Beispielprojekt platzieren Sie die Datei im Verzeichnis &quot;src&quot;.

![Screenshot des src-Verzeichnisses](assets/report_4.png)

Nachdem Sie die Anmeldeinformationen eingerichtet haben, können Sie die PDF-Konvertierungsaufgabe schreiben. Für diese Demonstration stehen Ihnen zwei Vorgänge zur Verfügung, die Sie in der Anwendung ausführen müssen:

* RAW-Dokumente in PDF-Dateien konvertieren

* Mehrere PDF-Dateien in einem Bericht kombinieren

Die allgemeine Vorgehensweise ist für die Ausführung eines Vorgangs ähnlich. Der einzige Unterschied besteht in dem Dienst, den Sie verwenden. Im folgenden Code konvertieren Sie das RAW-Dokument in eine PDF-Datei:

```
async function createPdf(rawFile, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("./src/pdftools-api-credentials.json")
.build();
// Capture the credential from app and show create the context
const executionContext = adobe.ExecutionContext.create(credentials),
operation = adobe.CreatePDF.Operation.createNew();
// Pass the content as input (stream)
const input = adobe.FileRef.createFromLocalFile(rawFile);
operation.setInput(input);
// Async create the PDF
let result = await operation.execute(executionContext);
await result.saveAsFile(outputPdf);
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

Im obigen Code lesen Sie die Anmeldeinformationen und erstellen den Ausführungskontext. Das PDF Services SDK erfordert den Ausführungskontext, um Ihre Anforderungen zu authentifizieren.

Anschließend führen Sie den Vorgang PDF erstellen aus, mit dem die RAW-Dokumente in das PDF-Format konvertiert werden. Zuletzt verwenden Sie den `outputPdf`-Parameter, um den PDF-Bericht zu kopieren. Im Codebeispiel finden Sie diesen Code unter der Datei src/helpers/pdf.js. Später in diesem Tutorial importieren Sie das PDF-Modul und rufen diese Methode auf.

Wie im vorherigen Abschnitt gezeigt, können Ihre Kunden auf die folgende Seite gehen, um die Berichte auszuwählen, die sie in PDF konvertieren möchten:

![Screenshot der Kundenfunktionalität](assets/report_3.png)

Wenn ein Kunde einen oder mehrere dieser Berichte auswählt, erstellen Sie die PDF-Datei.

Als Erstes zeige ich Ihnen eine einzelne PDF-Datei. Wenn der Benutzer einen einzelnen Bericht auswählt, müssen Sie ihn nur in PDF konvertieren und den Download-Link bereitstellen.

```
try {
console.log(`[INFO] generating the report...`);
await pdf.createPdf(`./public/documents/raw/${reports}`, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the report...`);
res.status(200).render("download", { page: 'reports', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

Dieser Code erstellt einen Bericht und gibt die Download-URL für den Kunden frei. Hier ist die Ausgabe-Webseite:

![Screenshot des Downloadbildschirms des Kunden](assets/report_5.png)

Und hier ist die Ausgabe-PDF:

![Screenshot des generischen Berichts](assets/report_6.png)

Kunden können mehrere Dateien auswählen, um einen kombinierten Bericht zu erstellen. Wenn der Kunde mehr als ein Dokument auswählt, führen Sie zwei Vorgänge aus: Erstens erstellt er eine partielle PDF für jedes Dokument, und zweitens fasst er diese in einem einzigen PDF-Bericht zusammen.

```
async function combinePdf(pdfs, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("./src/pdftools-api-credentials.json")
.build();
// Capture the credential from app and show create the context
const executionContext = adobe.ExecutionContext.create(credentials),
operation = adobe.CombineFiles.Operation.createNew();
// Pass the PDF content as input (stream)
for (let pdf of pdfs) {
const source = adobe.FileRef.createFromLocalFile(pdf);
operation.addInput(source);
}
// Async create the PDF
let result = await operation.execute(executionContext);
await result.saveAsFile(outputPdf);
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

Diese Methode ist unter der Datei src/helpers/pdf.js verfügbar und wird als Teil des Modulexports angezeigt.

```
try {
console.log(`[INFO] creating a batch report...`);
// Create a batch report and send it back
let partials = [];
for (let index in reports) {
const name = `partial-${index}-${reports[index]}`;
await pdf.createPdf(`./public/documents/raw/${reports[index]}`, `./public/documents/processed/${name}`);
partials.push(`./public/documents/processed/${name.replace('docx', 'pdf').replace('xlsx', 'pdf')}`);
}
await pdf.combinePdf(partials, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the combined report...`);
res.status(200).render("download", { page: 'reports', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

Dieser Code generiert einen kompilierten Bericht für mehrere Eingabedokumente. Die einzige hinzugefügte Funktion ist die `combinePdf`-Methode, die eine Liste von PDF-Dateipfadnamen verwendet und eine einzelne Ausgabe-PDF zurückgibt.

Jetzt können eure Social-Media-Dashboard-Kunden relevante Berichte aus ihrem Konto auswählen und als eine praktische PDF herunterladen. Mit diesem Dashboard können sie dem Management und anderen Stakeholdern den Erfolg ihrer Kampagnen mit Daten, Tabellen und Diagrammen in einem universell leicht zu öffnenden Format anzeigen.

## Nächste Schritte

In diesem praktischen Tutorial erfahren Sie, wie Sie die PDF Services-API verwenden, um Kunden beim Herunterladen relevanter Berichte als PDF zu unterstützen, die sich leicht weitergeben lassen. Sie haben eine Node.js-Anwendung erstellt, um die Leistungsfähigkeit der PDF Services-API für PDF-Berichts- und -Lesedienste zu demonstrieren. Die Anwendung zeigte, wie Ihre Kunden ein einzelnes Berichtsdokument herunterladen oder mehrere Dokumente zusammenführen und zu einem einzigen PDF-Bericht zusammenführen.

Mit dieser Adobe-basierten Anwendung können [Social-Media-Dashboard-Kunden](https://developer.adobe.com/document-services/use-cases/content-publishing/on-demand-report-creation) die benötigten Berichte erhalten und freigeben, ohne sich Gedanken darüber machen zu müssen, ob auf allen Empfängern Microsoft Office oder andere Software installiert ist. Sie können die gleichen Techniken in Ihrer eigenen Anwendung verwenden, um die Benutzer beim Anzeigen, Kombinieren und Herunterladen von Dokumenten zu unterstützen. Oder sehen Sie sich die vielen anderen APIs der Adobe an, um Signaturen hinzuzufügen und zu verfolgen und vieles mehr.

Fordern Sie zunächst Ihr kostenloses [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)-Konto an und erstellen Sie ansprechende Berichterlebnisse für Ihre Mitarbeiter und Kunden. Genießen Sie Ihr Konto sechs Monate lang kostenlos und dann [Pay-as-you-go](https://developer.adobe.com/document-services/pricing/main), wenn Ihre Marketing-Maßnahmen ausgebaut werden, nur \$0,05 pro Dokumenttransaktion.
