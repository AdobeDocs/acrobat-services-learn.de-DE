---
title: Angebote und Verträge verwalten
description: Erfahrt, wie ihr einen effizienten Workflow zur Automatisierung und Vereinfachung von Verkaufsangeboten entwickeln könnt.
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8099.jpg
jira: KT-8099
exl-id: 219c70de-fec1-4946-b10e-8ab5812562ef
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1395'
ht-degree: 2%

---

# Verwaltung von Verkaufsangeboten und Verträgen

![Use Case Hero-Banner](assets/UseCaseSalesHero.jpg)

Verkaufsangebote sind der erste Schritt auf dem Weg eines Unternehmens zur Kundenakquise. Wie bei allem, der erste Eindruck dauert. Ihre erste Interaktion mit Kunden bestimmt also deren Erwartungen an Ihr Unternehmen. Ihr Vorschlag muss präzise, präzise und bequem sein.

Verträge und Angebote enthalten verschiedene Datentypen innerhalb ihrer Dokumentstruktur. Sie enthalten sowohl dynamische Daten (Kundenname, Angebotsmenge usw.) als auch statische Daten (Standardtext wie feste Funktionen, Teamprofile und Standard-Leistungsbeschreibung). Das Erstellen von Vorlagendokumenten, z. B. Verkaufsangebote, erfordert oft monotone Aufgaben, z. B. das manuelle Ersetzen von Projektdetails in einer Mustervorlage. In diesem Tutorial verwenden Sie dynamische Daten und Workflows, um einen effizienten Prozess für [Verkaufsangebote erstellen](https://www.adobe.io/apis/documentcloud/dcsdk/sales-proposals-and-contracts.html).

## Lernziel.

In diesem praktischen Tutorial lernen Sie, wie Sie dynamische Daten und Workflows mit verschiedenen Tools implementieren, von denen die wichtigsten [!DNL Adobe Acrobat Services] APIs. Mit diesen APIs werden Verkaufsangebote und Verträge für Sie und Ihr Unternehmen komfortabler. In diesem Tutorial werden praktische Techniken zum automatischen Erstellen, Zusammenführen und Anzeigen von PDF-Dokumenten veranschaulicht. Diese Aufgaben manuell auszuführen, ist zeitaufwendig und mühsam. Durch Nutzung der [!DNL Acrobat Services] APIs können Sie die für diese Aufgaben aufgewendete Zeit verkürzen.

## Relevante APIs und Ressourcen

* [Microsoft Word](https://www.office.com/)

* [Node.js](https://nodejs.org/en/)

* [npm](https://www.npmjs.com/get-npm)

* [[!DNL Acrobat Services] APIs](https://www.adobe.io/apis/documentcloud/dcsdk/)

* [Adobe Dokumentenerzeugung API](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [Adobe Sign-API](https://www.adobe.io/apis/documentcloud/sign.html)

* [Adobe > Dokumenterstellung > Tagger](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

## Lösung des Problems

Nachdem Sie die Tools installiert haben, können Sie mit der Lösung des Problems beginnen. Die Angebote enthalten sowohl statischen als auch dynamischen Inhalt, der für jeden Kunden spezifisch ist. Engpässe treten auf, weil bei jedem Angebot beide Datentypen erforderlich sind. Die Eingabe des statischen Texts ist zeitaufwendig. Sie müssen ihn also automatisieren und nur manuell mit den dynamischen Daten der einzelnen Clients umgehen.

Erstellen Sie zunächst ein Datenerfassungsformular in [Microsoft Forms](https://www.office.com/launch/forms?auth=1) (oder von deinem bevorzugten Formularersteller). Dieses Formular ist für dynamische Kundendaten gedacht, die einem Verkaufsangebot hinzugefügt werden. Fülle das Formular mit Fragen aus, um die gewünschten Kundendetails zu erhalten, z. B. Firmenname, Datum, Adresse, Projektumfang, Preise und zusätzliche Kommentare. Verwenden Sie diese [Form](https://forms.office.com/Pages/ShareFormPage.aspx). Das Ziel ist es, dass potenzielle Kunden das Formular ausfüllen und ihre Antworten dann als JSON-Dateien exportieren, die an den nächsten Teil Ihres Workflows übergeben werden.

Bei einigen Formularerstellern können Sie Daten nur als CSV-Dateien exportieren. Vielleicht finden Sie es nützlich, [konvertieren](http://csvjson.com/csv2json) die generierte CSV-Datei in eine JSON-Datei um.

Die statischen Daten werden in jedem Verkaufsangebot wiederverwendet. Sie können also eine Vorlage für ein Verkaufsangebot in Microsoft Word verwenden, um den statischen Text bereitzustellen. Sie können dieses [Vorlage](https://1drv.ms/w/s!AiqaN2pp7giKkmhVu2_2pId9MiPa?e=oeqoQ2), aber Sie können Ihre eigenen erstellen oder ein [Vorlage für Adoben](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html).

Sie benötigen jetzt etwas, das sowohl die dynamischen Daten der Kunden im JSON-Format als auch den statischen Text in der Microsoft Word-Vorlage zu einem individuellen Verkaufsvorschlag für einen Kunden zusammenfasst. Die [!DNL Acrobat Services] APIs werden verwendet, um die beiden zusammenzuführen und eine PDF zu generieren, die signiert werden kann.

Damit das funktioniert, verwenden Sie Tags. Tags sind benutzerfreundliche Zeichenfolgen, die Zahlen, Wörter, Arrays oder sogar komplexe Objekte darstellen können. Tags dienen als Platzhalter für dynamische Daten, in diesem Fall Clientdaten, die in das Formular eingegeben werden. Nachdem Sie Tags in die Vorlage eingefügt haben, können Sie Formularfelder aus der JSON-Datei der Word-Vorlage zuordnen.

## Tag-Verwendung

Öffnen Sie Ihre Vorlage für Verkaufsangebote und wählen Sie das **Einfügen** &quot; ändern. Im Dialogfeld **Add-ins** Gruppe wählen Sie **Add-ins abrufen**. Wählen Sie dann **Add-In für die Generierung von Adobe-Dokumenten** , um sie hinzuzufügen. Nach dem Hinzufügen wird das Tagger für die Dokumentgenerierung auf der Seite **Startseite** &quot; im Dialogfeld &quot; **Adobe** Gruppe.

Im Fenster &quot; **Startseite** &quot; im Dialogfeld &quot; **Adobe** Gruppe wählen Sie **Dokumenterstellung** , um mit dem Taggen des Dokuments zu beginnen. Ein hilfreiches Demonstrationsvideo wird in einem Bedienfeld auf der rechten Seite des Fensters angezeigt.

![Screenshot des Add-Ins &quot;Dokument-Tagger&quot; in Word](assets/sales_1.png)

Auswählen **Erste Schritte**. Sie werden dann aufgefordert, Beispieldaten bereitzustellen. Fügen Sie die JSON-Datei mit der Formularantwort wie unten gezeigt ein oder laden Sie sie hoch.

![Screenshot des Einfügens des Beispielcodes](assets/sales_2.png)

Auswählen **Tags generieren** , um eine Liste der Felder aus der JSON-Datei abzurufen, die Sie eingefügt oder hochgeladen haben. Die Tags werden unten auf der rechten Seitenleiste angezeigt.

![Screenshot der verfügbaren Tags](assets/sales_3.png)

Nach dem Generieren der Tags können Sie sie in das Dokument einfügen. Tags werden an der Cursorposition zum Dokument hinzugefügt. Wie oben gezeigt, sollten Sie die **Projektumfang** Tag direkt unter dem **Projektumfang** Untertitel. Auf diese Weise wird die Antwort eines Kunden, der in das Formular in den Projektbereich eintritt, unter die **Projektumfang** -Untertitel, der das soeben hinzugefügte Tag ersetzt. Nachdem Sie die Tags hinzugefügt haben, sollte ein Teil des Dokuments wie die Bildschirmaufnahme unten aussehen.

![Screenshot des Hinzufügens von Tags zu einem Word-Dokument](assets/sales_4.png)

## APIs verwenden

Wechseln Sie zur Registerkarte [!DNL Acrobat Services] APIs [Homepage](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html). So verwenden Sie [!DNL Acrobat Services] APIs benötigen Sie Anmeldeinformationen für Ihre Anwendung. Scrollen Sie ganz nach unten und wählen Sie **Kostenlos testen** , um Anmeldeinformationen zu erstellen. Sie können diese Dienste verwenden [sechs Monate lang kostenlos, dann Pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) für nur 0,05 US-Dollar pro Dokumenttransaktion, sodass Sie nur das bezahlen, was Sie benötigen.

Auswählen **PDF Services API** als Ihren bevorzugten Service an und füllen Sie die anderen Details wie unten gezeigt aus.

![Screenshot mit Anmeldeinformationen](assets/sales_5.png)

Nachdem Sie Ihre Anmeldeinformationen erstellt haben, erhalten Sie einige Codebeispiele. Wählen Sie Ihre bevorzugte Sprache (in diesem Tutorial wird Node.js verwendet). Ihre API-Zugangsberechtigungen befinden sich in einer ZIP-Datei. Extrahieren Sie die Dateien in PDFToolsSDK-Node.jsSamples.

Erstellen Sie zunächst einen leeren Ordner mit dem Namen auto-doc\*\*.\*\* Führen Sie im Ordner den folgenden Befehl aus, um ein Projekt &quot;Node.js&quot; zu initialisieren: `npm init`. Nennen Sie Ihr Projekt &quot;auto-doc&quot;*.*

Im Ordner ./PDFToolsSDK-Node.jsSamples/adobe-dc-pdf-tools-sdk-node-samples, gibt es eine Datei namens pdftools-api-credentials.json. Verschieben Sie ihn und &quot;private.key&quot; in den Ordner &quot;auto-doc&quot;. Sie enthält Ihre API-Zugangsberechtigungen. Erstellen Sie außerdem im Ordner für automatische Dokumente einen Unterordner mit dem Namen &quot;resources&quot;. Es enthält die JSON-formatierten Daten, die von Kunden empfangen werden, wenn Sie ein Verkaufsangebot erstellen. Speichern Sie im selben Ordner die Vorlage für Verkaufsangebote aus Microsoft Word.

Jetzt kannst du zaubern! Da Sie in diesem Tutorial die Datei &quot;Node.js&quot; verwenden, müssen Sie die Datei &quot;Node.js&quot; installieren. [!DNL Acrobat Services] SDK Führen Sie dazu im Ordner &quot;Auto-doc&quot; die Datei &quot;thread add @adobe/documentservices-pdftools-node-sdk&quot; aus.

Erstellen Sie nun eine Datei namens merge.js und fügen Sie den folgenden Code ein.

```
javascript
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk'),
fs = require('fs');
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Setup input data for the document merge process
const jsonString = fs.readFileSync('resources/Proposal.json'),
jsonDataForMerge = JSON.parse(jsonString);
// Create an ExecutionContext using credentials
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
// Create a new DocumentMerge options instance
const documentMerge = PDFToolsSdk.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);
// Create a new operation instance using the options instance
const documentMergeOperation = documentMerge.Operation.createNew(options)
// Set operation input document template from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile('resources/Proposal.docx');
documentMergeOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
.then(result => result.saveAsFile('output/Proposal.pdf'))
.catch(err => {
if (err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log('Exception encountered while executing operation', err);
} else {
console.log('Exception encountered while executing operation', err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
```

Dieser Code ruft Ihre JSON-Datei aus dem Microsoft-Formular mithilfe der Tags ab, die Sie mit [!DNL Acrobat Services]. Anschließend werden die Daten mit der in Microsoft Word erstellten Vorlage für Verkaufsangebote zusammengeführt, um eine brandneue PDF zu erstellen. Die PDF wird im neu erstellten gespeichert./Ausgabeordner.

Außerdem verwendet der Code [Adobe Sign API](https://www.adobe.io/apis/documentcloud/sign.html) , damit beide Unternehmen das Angebot unterzeichnen. In diesem Blogpost finden Sie eine detaillierte Erklärung zu dieser API.

## Nächste Schritte

Sie begannen mit einem ineffizienten, langwierigen Prozess, der Automatisierung erforderte. Von der manuellen Erstellung von Dokumenten für jeden Kunden zur Erstellung eines optimierten Workflows zur Automatisierung und Vereinfachung [das Angebotsverfahren](https://www.adobe.io/apis/documentcloud/dcsdk/sales-proposals-and-contracts.html).

Mit Microsoft Forms erhaltet ihr wichtige Daten von euren Kunden für ihre individuellen Angebote. Sie haben in Microsoft Word eine Vorlage für ein Verkaufsangebot erstellt, die den statischen Text enthält, den Sie nicht jedes Mal neu erstellen möchten. Sie haben dann [!DNL Acrobat Services] APIs zum Zusammenführen von Daten aus dem Formular und der Vorlage, um eine Verkaufsangebot-PDF für Ihre Kunden auf effizientere Weise zu erstellen.

In diesem Tutorial erfahren Sie, was mit diesen APIs alles möglich ist. Weitere Lösungen finden Sie auf der Seite [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) APIs-Seite. Nutze diese Tools 6 Monate lang kostenlos. Dann zahlen Sie nur $0.05 pro Dokumenttransaktion auf der [pay as you go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) daher zahlen Sie nur, wenn Ihr Team Ihre Vertriebspipeline um weitere Interessenten erweitert.
