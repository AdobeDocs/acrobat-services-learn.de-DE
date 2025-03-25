---
title: Angebote und Verträge verwalten
description: Erfahrt, wie ihr einen effizienten Workflow zur Automatisierung und Vereinfachung von Verkaufsangeboten entwickeln könnt.
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8099
thumbnail: KT-8099.jpg
exl-id: 219c70de-fec1-4946-b10e-8ab5812562ef
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1306'
ht-degree: 0%

---

# Verwaltung von Verkaufsangeboten und Verträgen

![Banner &quot;Use Case Hero&quot;](assets/UseCaseSalesHero.jpg)

Verkaufsangebote sind der erste Schritt auf dem Weg eines Unternehmens zur Kundenakquise. Wie bei allem, der erste Eindruck dauert. Ihre erste Interaktion mit Kunden bestimmt also deren Erwartungen an Ihr Unternehmen. Ihr Vorschlag muss präzise, präzise und bequem sein.

Verträge und Angebote enthalten verschiedene Datentypen innerhalb ihrer Dokumentstruktur. Sie enthalten sowohl dynamische Daten (Kundenname, Angebotsmenge usw.) als auch statische Daten (Standardtexte wie feste Funktionen, Teamprofile und Standard-Leistungsbeschreibung). Das Erstellen von Vorlagendokumenten, z. B. Verkaufsangebote, erfordert oft monotone Aufgaben, z. B. das manuelle Ersetzen von Projektdetails in einer Mustervorlage. In diesem Tutorial verwenden Sie dynamische Daten und Workflows, um einen effizienten Prozess für die [Erstellung von Verkaufsangeboten](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/sales-proposals-and-contracts) zu erstellen.

## Lernziel.

In diesem praktischen Tutorial lernen Sie, wie Sie dynamische Daten und Workflows mit verschiedenen Tools implementieren, von denen die wichtigsten [!DNL Adobe Acrobat Services]-APIs sind. Mit diesen APIs werden Verkaufsangebote und Verträge für Sie und Ihr Unternehmen komfortabler. In diesem Tutorial werden praktische Techniken zum automatischen Erstellen, Zusammenführen und Anzeigen von PDF-Dokumenten veranschaulicht. Diese Aufgaben manuell auszuführen, ist zeitaufwendig und mühsam. Wenn Sie die [!DNL Acrobat Services] APIs nutzen, können Sie die Zeit für diese Aufgaben verkürzen.

## Relevante APIs und Ressourcen

* [Microsoft Word](https://www.office.com/)

* [Node.js](https://nodejs.org/en/)

* [npm](https://www.npmjs.com/get-npm)

* [[!DNL Acrobat Services] APIs](https://developer.adobe.com/document-services/homepage/)

* [API für die Dokumentenerzeugung ](https://developer.adobe.com/document-services/apis/doc-generation) für Adobe

* [Adobe Sign-API](https://developer.adobe.com/adobesign-api/)

* [Adobe-Tagger für Dokumenterstellung](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

## Lösung des Problems

Nachdem Sie die Tools installiert haben, können Sie mit der Lösung des Problems beginnen. Die Angebote enthalten sowohl statischen als auch dynamischen Inhalt, der für jeden Kunden spezifisch ist. Engpässe treten auf, weil bei jedem Angebot beide Datentypen erforderlich sind. Die Eingabe des statischen Texts ist zeitaufwendig. Sie müssen ihn also automatisieren und nur manuell mit den dynamischen Daten der einzelnen Clients umgehen.

Erstellen Sie zunächst ein Datenerfassungsformular in [Microsoft Forms](https://www.office.com/launch/forms?auth=1) (oder in Ihrem bevorzugten Formularersteller). Dieses Formular ist für dynamische Kundendaten gedacht, die zu einem Verkaufsangebot hinzugefügt werden. Fülle das Formular mit Fragen aus, um die benötigten Kundendaten abzurufen, z. B. Firmenname, Datum, Adresse, Projektumfang, Preise und zusätzliche Kommentare. Verwenden Sie zum Erstellen Ihres eigenen Formulars [form]&#x200B;(https://forms.office.com/Pages/ShareFormPage.aspx id=DQSIkWdsW0yxEjajBLZtrQAAAAAAAAAAAAAAN__rtiGj5UNElTR0pCQ09ZNkJRUlowSjVQWDNYUEg2RC4u&amp;sharetoken=1AJeMavBAzzxuISRKmUy). Das Ziel ist es, dass potenzielle Kunden das Formular ausfüllen und ihre Antworten dann als JSON-Dateien exportieren, die an den nächsten Teil Ihres Workflows übergeben werden.

Manche Formularersteller bieten nur die Möglichkeit, Daten als CSV-Dateien zu exportieren. Daher ist es möglicherweise nützlich, [die generierte CSV-Datei in eine JSON-Datei zu konvertieren](http://csvjson.com/csv2json).

Die statischen Daten werden in jedem Verkaufsangebot wiederverwendet. Sie können also eine Vorlage für ein Verkaufsangebot in Microsoft Word verwenden, um den statischen Text bereitzustellen. Sie können diese [Vorlage](https://1drv.ms/w/s!AiqaN2pp7giKkmhVu2_2pId9MiPa?e=oeqoQ2) verwenden, aber Sie können Ihre eigene Vorlage erstellen oder eine [Adobe-Vorlage](https://developer.adobe.com/document-services/apis/doc-generation) verwenden.

Sie benötigen jetzt etwas, das sowohl die dynamischen Daten der Kunden im JSON-Format als auch den statischen Text in der Microsoft Word-Vorlage zu einem individuellen Verkaufsvorschlag für einen Kunden zusammenfasst. Die [!DNL Acrobat Services]-APIs werden verwendet, um die beiden zusammenzuführen und eine PDF zu generieren, die signiert werden kann.

Damit das funktioniert, verwenden Sie Tags. Tags sind benutzerfreundliche Zeichenfolgen, die Zahlen, Wörter, Arrays oder sogar komplexe Objekte darstellen können. Tags dienen als Platzhalter für dynamische Daten, in diesem Fall Clientdaten, die in das Formular eingegeben werden. Nachdem Sie Tags in die Vorlage eingefügt haben, können Sie Formularfelder aus der JSON-Datei der Word-Vorlage zuordnen.

## Verwenden von Tags

Öffnen Sie Ihre Vorlage für Verkaufsangebote, und wählen Sie die Registerkarte **Einfügen** aus. Wählen Sie in der Gruppe **Add-Ins** die Option **Add-Ins abrufen**. Wählen Sie dann das Add-In **Adobe-Dokumentengenerierung** aus, um es hinzuzufügen. Nach dem Hinzufügen wird der Tagger für die Dokumentgenerierung auf der Registerkarte **Startseite** in der Gruppe **Adobe** angezeigt.

Wählen Sie auf der Registerkarte **Startseite** in der Gruppe **Adobe** die Option **Dokumenterstellung** aus, um mit dem Taggen des Dokuments zu beginnen. Ein hilfreiches Demonstrationsvideo wird in einem Bedienfeld auf der rechten Seite des Fensters angezeigt.

![Screenshot des Add-Ins &quot;Dokument-Tagger&quot; in Word](assets/sales_1.png)

Wählen Sie **Erste Schritte**. Sie werden dann aufgefordert, Beispieldaten bereitzustellen. Fügen Sie die JSON-Datei mit der Formularantwort wie unten gezeigt ein oder laden Sie sie hoch.

![Screenshot des Einfügens des Beispielcodes](assets/sales_2.png)

Wählen Sie **Tags generieren**, um eine Liste von Feldern aus der eingefügten oder hochgeladenen JSON-Datei abzurufen. Die Tags werden unten auf der rechten Seitenleiste angezeigt.

![Screenshot der verfügbaren Tags](assets/sales_3.png)

Nach dem Generieren der Tags können Sie sie in das Dokument einfügen. Tags werden an der Cursorposition zum Dokument hinzugefügt. Wie oben gezeigt, sollten Sie das Tag **Projektumfang** direkt unter dem Untertitel **Projektumfang** hinzufügen. Wenn ein Client also den Projektbereich im Formular eingibt, geht seine Antwort unter den Untertitel **Projektbereich** und ersetzt das soeben hinzugefügte Tag. Nachdem Sie die Tags hinzugefügt haben, sollte ein Teil des Dokuments wie die Bildschirmaufnahme unten aussehen.

![Screenshot des Hinzufügens von Tags zum Word-Dokument](assets/sales_4.png)

## Verwenden der APIs

Wechseln Sie zu den [!DNL Acrobat Services] APIs [Homepage](https://developer.adobe.com/document-services/apis/doc-generation). Um [!DNL Acrobat Services] APIs verwenden zu können, benötigen Sie Anmeldeinformationen für Ihre Anwendung. Scrollen Sie ganz nach unten und wählen Sie **Kostenlose Testversion starten**, um Anmeldeinformationen zu erstellen. Sie können diese Dienste [sechs Monate lang kostenlos nutzen und dann für nur 0,05 US-Dollar pro Dokumenttransaktion nach Bedarf](https://developer.adobe.com/document-services/pricing/main) bezahlen, sodass Sie nur das bezahlen, was Sie benötigen.

Wählen Sie **PDF Services API** als Ihren bevorzugten Dienst aus und geben Sie die anderen Informationen wie unten gezeigt ein.

![Screenshot des Abrufens der Anmeldeinformationen](assets/sales_5.png)

Nachdem Sie Ihre Anmeldeinformationen erstellt haben, erhalten Sie einige Codebeispiele. Wählen Sie Ihre bevorzugte Sprache aus (in diesem Tutorial wird Node.js verwendet). Ihre API-Zugangsberechtigungen befinden sich in einer ZIP-Datei. Extrahieren Sie die Dateien in PDFToolsSDK-Node.jsSamples.

Erstellen Sie zunächst einen leeren Ordner mit dem Namen auto-doc\*\*.\*\* Führen Sie im Ordner den folgenden Befehl aus, um ein Projekt &quot;Node.js&quot; zu initialisieren: `npm init`. Nennen Sie Ihr Projekt &quot;auto-doc&quot;*.*

Im Ordner ./PDFToolsSDK-Node.jsSamples/adobe-dc-pdf-tools-sdk-node-samples, gibt es eine Datei namens pdftools-api-credentials.json. Verschieben Sie ihn und &quot;private.key&quot; in den Ordner &quot;auto-doc&quot;. Sie enthält Ihre API-Zugangsberechtigungen. Erstellen Sie außerdem im Ordner für automatische Dokumente einen Unterordner mit dem Namen &quot;resources&quot;. Es enthält die JSON-formatierten Daten, die von Kunden empfangen werden, wenn Sie ein Verkaufsangebot erstellen. Speichern Sie im selben Ordner die Vorlage für Verkaufsangebote aus Microsoft Word.

Jetzt kannst du zaubern! Da Sie in diesem Tutorial &quot;Node.js&quot; verwenden, müssen Sie das SDK &quot;Node.js [!DNL Acrobat Services]&quot; installieren. Führen Sie dazu im Ordner &quot;Auto-doc&quot; die Datei &quot;thread add @adobe/documentservices-pdftools-node-sdk&quot; aus.

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

Dieser Code ruft Ihre JSON-Datei mithilfe der Tags, die Sie mit [!DNL Acrobat Services] erstellt haben, aus dem Microsoft-Formular ab. Anschließend werden die Daten mit der von Ihnen in Microsoft Word erstellten Vorlage für Verkaufsangebote zusammengeführt, um eine brandneue PDF zu erstellen. Die PDF wird im neu erstellten gespeichert./Ausgabeordner.

Außerdem verwendet der Code die [Adobe Sign-API](https://developer.adobe.com/adobesign-api/), damit beide Unternehmen das generierte Verkaufsangebot unterzeichnen. In diesem Blogpost finden Sie eine detaillierte Erklärung zu dieser API.

## Nächste Schritte

Am Anfang stand ein ineffizienter, mühsamer Prozess, der der Automatisierung bedurfte. Sie sind von der manuellen Erstellung von Dokumenten für jeden Client zur Erstellung eines optimierten Workflows übergegangen, um [den Vertriebsangebot-Prozess zu automatisieren und zu vereinfachen](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/sales-proposals-and-contracts).

Mit Microsoft Forms erhaltet ihr wichtige Daten von euren Kunden, die in ihre individuellen Vorschläge einfließen. Sie haben in Microsoft Word eine Vorlage für ein Verkaufsangebot erstellt, die den statischen Text enthält, den Sie nicht jedes Mal neu erstellen möchten. Sie haben dann [!DNL Acrobat Services] APIs verwendet, um Daten aus dem Formular und der Vorlage zusammenzuführen, um eine Verkaufsangebot-PDF für Ihre Kunden auf effizientere Weise zu erstellen.

In diesem Tutorial erfahren Sie, was mit diesen APIs alles möglich ist. Weitere Lösungen finden Sie auf der Seite [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) APIs. Nutze diese Tools 6 Monate lang kostenlos. Dann zahlen Sie nur 0,05 US-Dollar pro Dokumenttransaktion für das [Pay-as-you-go](https://developer.adobe.com/document-services/pricing/main)-Abo, sodass Sie nur zahlen, wenn Ihr Team mehr Interessenten zu Ihrer Vertriebspipeline hinzufügt.
