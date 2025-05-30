---
title: Erstellen einer Geheimhaltungsvereinbarung
description: Erfahren Sie, wie Sie eine dynamische PDF mit Vertraulichkeitsvereinbarung für Zusammenarbeit erstellen.
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8098
thumbnail: KT-8098.jpg
exl-id: f4ec0182-a46e-43aa-aea3-bf1d19f1a4ec
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1072'
ht-degree: 0%

---

# Erstellen einer Geheimhaltungsvereinbarung

![Banner &quot;Use Case Hero&quot;](assets/UseCaseNDAHero.jpg)

Organisationen arbeiten mit externen Anbietern zusammen, um ihre Dienste und Produkte aufzubauen. Eine Vertraulichkeitsvereinbarung ist eine wichtige dieser Kooperationen. Alle Parteien sind verpflichtet, vertrauliche Informationen, die einer der beiden Parteien schaden könnten, weiterzugeben.

Das am häufigsten verwendete Format für eine Geheimhaltungsvereinbarung ist ein PDF-Dokument. Organisationen bereiten eine Geheimhaltungsvereinbarung vor und senden sie an alle Parteien. Sobald alle unterzeichnet haben, initiieren sie den Vertrag. In einem Hochgeschwindigkeits-Team verlangsamt die manuelle PDF-Erstellung den Fortschritt.

## Lernziel.

In diesem Tutorial erfährst du, wie du eine Microsoft Word-Vorlage für deine Vertraulichkeitsvereinbarung erstellst. Das kostenlose Add-In von Adobe für Microsoft Word, [Adobe Document Generation Tagger](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo), fügt &quot;Tags&quot; ein, um die dynamischen Werte einzugeben. Erfahren Sie, wie Sie die JSON-Daten an die Vorlage übergeben und eine dynamische PDF erstellen. Die resultierende PDF kann je nach Ihren Geschäftsanforderungen und -zielen per E-Mail an Ihre Mitarbeiter in deren Browser gesendet oder angezeigt werden. Um die Schritte in diesem Tutorial besser nachvollziehen zu können, brauchst du nur ein wenig Erfahrung mit Node.js, JavaScript, Express.js, HTML und CSS.

## Relevante APIs und Ressourcen

Mit [!DNL Adobe Acrobat Services] können Sie PDF-Dokumente mithilfe dynamischer Daten im laufenden Betrieb generieren. [!DNL Acrobat Services] bietet eine Reihe von PDF-Tools, einschließlich der Adobe-API zum Generieren von Dokumenten, um die [Erstellung von Geheimhaltungsvereinbarungen](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/nda-creation) zu automatisieren.

* [API für die Dokumentenerzeugung ](https://developer.adobe.com/document-services/apis/doc-generation) für Adobe

* [Adobe Sign-API](https://developer.adobe.com/adobesign-api/)

* [Adobe-Tagger für Dokumenterstellung](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

* [Projektcode](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample)

* [[!DNL Acrobat Services] Schlüssel](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred)

## Erstellen des JSON-Modells

Die Microsoft Word-Vorlage hängt vom JSON-Modell ab. Erstellen Sie diese daher zuerst. Für das Tutorial verwenden Sie eine grundlegende JSON-Struktur, die Unternehmensdetails enthält, wie z. B. Kontaktinformationen.

```
{
"vendor": {
"companyName": "GlobalCorp",
"street": "123 Any Street",
"street2": "",
"city":"Anywhere",
"state":"CA",
"primaryContact": {
"firstName":"John",
"lastName":"Doe",
"email":"john-doe@example.com",
"phone":"123-456-7890"
}
},
"authorizedSigner": {
"firstName": "Sarah",
"lastName": "Rose",
"email": "sarah@example.com",
"phone":"555-555-1234"
}
}
```

Verwenden Sie diese Struktur in Microsoft Word, um eine Vorlage zu generieren. Diese Daten können aus jeder beliebigen Datenquelle stammen, solange sie im JSON-Format vorliegen. Der Einfachheit halber erstellen Sie mehrere Dateien in der Anwendung Node.js, aber für Ihren Anwendungsfall ist möglicherweise eine Datenbankverbindung erforderlich, um Herstellerinformationen abzurufen.

## Erstellen der Microsoft Word-Vorlage

Erstellen Sie die Vorlage für die Geheimhaltungsvereinbarung in einem Microsoft Word-Dokument. Die Adobe PDF Services-API erwartet, dass das Microsoft Word-Dokument Tags enthält, in die der Dienst Werte aus JSON-Dokumenten einfügen kann. Obwohl die Vorlage für alle Anforderungen zum Adobe identisch ist, ändern sich die dynamischen Daten in JSON. Diese Tags helfen dabei, PDF-Dokumente für jeden Hersteller in diesem Fall mit einer einzigen Microsoft Word-Vorlage zu erstellen und den Prozess durch die Automatisierung der Generierung von Dokumenten mit Geheimhaltungsvereinbarung zu beschleunigen.

Sie können das [kostenlose Add-in für Tagger für die Dokumentenerzeugung](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo) in Microsoft Word installieren. Wenn Sie Teil eines Unternehmens sind, können Sie Ihren Microsoft Office-Administrator bitten, das kostenlose Add-in für alle zu installieren.

Sobald Sie das Add-In installiert haben, finden Sie es auf der Registerkarte Startseite unter der Kategorie Adobe. Um die Registerkarte zu öffnen, wählen Sie **Dokumentenerzeugung**:

![Screenshot des Add-Ins für die Dokumentengenerierung in Word](assets/nda_1.png)

Auf der Registerkarte können Sie das Beispiel-JSON-Dokument hochladen. Dieses Dokument kann ein Beispiel sein, da Sie es nur zum Erstellen einer Microsoft Word-Vorlage verwenden.

![Screenshot der Beispieldaten im Add-in &quot;Dokumentenerzeugung&quot;](assets/nda_2.png)

Wählen Sie **Tags generieren**, um Elemente anzuzeigen, die Sie in Ihrer Vorlage verwenden können. Hier sind die Eigenschaften, die aus der JSON-Struktur extrahiert wurden und in der Vorlage verwendet werden können:

![Screenshot der Text-Tags im Add-in für die Dokumentenerzeugung](assets/nda_3.png)

Dies sind die Features aus dem Feld `authorizedSigner`. Andere Felder sind umgebrochen und Sie können die Ansicht in Microsoft Word erweitern. Das Add-In bietet auch erweiterte Datenoptionen wie Tabellen, Listen, berechnete Werte und mehr.

## Erstellen der Tags

Erstellen Sie eine Vorlage, oder importieren Sie eine [vorhandene Vorlage](https://developer.adobe.com/document-services/apis/doc-generation#sample-blade) in Microsoft Word. Nachdem Sie das Dokument eingerichtet haben, fügen Sie den einzelnen Feldern Tags hinzu, indem Sie auf die entsprechenden Token im Add-In klicken.

Die folgende Vorlage in einer Microsoft Word-Datei:

![Screenshot der Beispielvorlage](assets/nda_4.png)

Diese Datei enthält mehrere Tags. Wenn Sie das Programm ausführen, werden diese Felder mit den Kreditoreninformationen gefüllt.

Der Tagger für die Dokumentenerzeugung ist in die Adobe Sign-API integriert. Dank dieser Integration können Sie Sign-Text-Tags automatisch erstellen, sodass das generierte Dokument dann zur Signatur an Adobe Sign gesendet werden kann.

## Generieren der Geheimhaltungsvereinbarung für Anbieter

Innerhalb der Beispielanwendung haben Sie Ordner für Eingabe und Ausgabe vorbereitet. Wie bereits erwähnt, verwenden Sie JSON-Dateien, sodass es zwei Dateien gibt, um die verfügbaren Anbieter im System anzuzeigen. Die Dateien werden in einem Formular angezeigt, das im Browser gedruckt wird:

```
<h1><b>NDA</b>: Generate for vendor.</h1>
<hr />
<p>Following ({{files.length}}) vendors are ready, select to generate NDA and deliver for signature:</p>
<form method="POST">
<ul>
{{#each files }}
<li><input type="checkbox" name="vendor" value="{{this}}" id="file-{{@index}}" /> <label for="file-{{@index}}">{{this}}</label></li>
{{/each}}
</ul>
<input type="submit" value="Create NDA" />
</form>
```

Dieser Code generiert die folgende Benutzeroberfläche im Browser:

![Screenshot der Benutzeroberfläche zum Erstellen einer Geheimhaltungsvereinbarung](assets/nda_5.png)

Wenn der Administrator eine Person auswählt, verwendet die App Adobe PDF Services, um die Geheimhaltungsvereinbarung unterwegs zu generieren.

```
async function compileDocFile(json, inputFile, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("./src/pdftools-api-credentials.json")
.build();
// Capture the credential from app and show create the context
const executionContext = adobe.ExecutionContext.create(credentials);
// create the operation
const documentMerge = adobe.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(json, documentMergeOptions.OutputFormat.PDF);
const operation = documentMerge.Operation.createNew(options);
// Pass the content as input (stream)
const input = adobe.FileRef.createFromLocalFile(inputFile);
operation.setInput(input);
// Async create the PDF
let result = await operation.execute(executionContext);
await result.saveAsFile(outputPdf);
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

Verwenden Sie diesen Code im Express-Router:

```
// Create one report and send it back
try {
console.log(`[INFO] generating the report...`);
const fileContent = fs.readFileSync(`./public/documents/raw/${vendor}`, 'utf-8');
const parsedObject = JSON.parse(fileContent);
await pdf.compileDocFile(parsedObject, `./public/documents/template/Adobe-NDA-Sample.docx`, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the report...`);
res.status(200).render("preview", { page: 'nda', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

Sie können [ den vollständigen Beispielcode ](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample) auf GitHub anzeigen.

Dieser Code verwendet ein JSON-Dokument und die Microsoft Word-Vorlage im API-Aufruf des [!DNL Adobe Acrobat Services]-SDK. In der Antwort erhalten Sie die Ausgabe und speichern sie im Dateisystem der App. Sie können das generierte Dokument per E-Mail an Ihre Kunden weiterleiten oder ihnen mithilfe der kostenlosen [Adobe PDF Embed-API](https://developer.adobe.com/document-services/apis/pdf-embed) eine Vorschau im Browser anzeigen.

Durch diesen Aufruf wird das folgende Dokument mit der Geheimhaltungsvereinbarung erstellt:

![Screenshot der Vorschau des Dokuments mit der Geheimhaltungsvereinbarung](assets/nda_6.png)

[!DNL Adobe Acrobat Services] APIs fügen Inhalt zum Erstellen eines PDF-Dokuments ein. Ohne diese Tools müssen Sie möglicherweise den Code schreiben, um Office-Dokumente zu verarbeiten und mit Raw-PDF-Dateiformaten zu arbeiten. Mithilfe von Adobe PDF Services können Sie all diese Schritte mit einem einzigen API-Aufruf ausführen.

Verwenden Sie jetzt [Adobe Sign API](https://developer.adobe.com/adobesign-api/), um Signaturen für die Geheimhaltungsvereinbarungen anzufordern und das endgültige, signierte Dokument an alle Parteien zu übermitteln. Adobe Sign benachrichtigt Sie [ mithilfe eines Webhooks ](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/webhooks.md). Wenn Sie diesem Webhook zuhören, können Sie den Status der Geheimhaltungsvereinbarung abrufen.

Eine ausführlichere Erläuterung des Adobe Sign-Prozesses finden Sie in [der Dokumentation](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html) oder in diesem ausführlichen Blogpost.

## Nächste Schritte

In diesem praktischen Tutorial wurde der Adobe-Tagger zum Generieren von Dokumenten verwendet, um PDF-Dokumente mithilfe von Microsoft Word-Vorlagen und JSON-Datendateien dynamisch zu generieren. Das Add-In half [, automatisch für jede Partei angepasste Vertraulichkeitsvereinbarungen zu erstellen](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/nda-creation) und dann Signaturen über die Sign-API zu erfassen.

Mit diesen Techniken könnt ihr eigene Vertraulichkeitsvereinbarungen oder andere Dokumente dynamisch erstellen. So haben eure Teams mehr Zeit für produktive Aufgaben. Erkunden Sie [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/apis/pdf-services), um APIs und SDKs für Ihre bevorzugte Sprache und Laufzeit zu finden, sodass Sie PDF-Funktionen direkt zu Ihren Anwendungen hinzufügen können, um schnell PDF-Dokumente zu erstellen. [Erste Schritte](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) mit einer sechsmonatigen kostenlosen Testversion
[Pay-as-you-go](https://developer.adobe.com/document-services/pricing/main) für nur 0,05 $ pro Dokumenttransaktion.
