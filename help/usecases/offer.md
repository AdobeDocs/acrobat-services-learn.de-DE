---
title: Verwalten von Mitarbeiterangeboten
description: Hier erfahren Sie, wie Sie ein Angebotsschreiben generieren, das einem neuen Mitarbeiter zum Unterschreiben zugestellt werden kann.
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8096.jpg
jira: KT-8096
exl-id: 92f955f0-add5-4570-aa3a-ea63055dadb2
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1794'
ht-degree: 2%

---

# Angebotsschreiben von Mitarbeitern verwalten

![Use Case Hero-Banner](assets/UseCaseOfferHero.jpg)

Angebotsschreiben von Mitarbeitern sind eine der ersten Erfahrungen, die Mitarbeiter mit Ihrer Organisation machen. Daher ist es wichtig, dass eure Angebotsschreiben zur Marke passen, aber ihr möchtet nicht jedes Mal einen Brief von Grund auf neu in eurem Textverarbeitungsprogramm erstellen müssen. [!DNL Adobe Acrobat Services] APIs bieten eine schnelle, einfache und effektive Möglichkeit, wichtige Teile der [Erstellen und Zustellen von Angebotsschreiben an neue Mitarbeiter](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html).

## Lernziel.

Dieses praktische Tutorial führt Sie durch die Einrichtung eines Node Express-Projekts, das ein Webformular für einen Benutzer anzeigt, um die Mitarbeiterdetails anzuzeigen. Diese Details verwenden [!DNL Acrobat Services] über das Internet, um ein Angebotsschreiben als PDF zu generieren, das mit der Adobe Sign-API an einen Kunden zur Signatur gesendet werden kann.

## Relevante APIs und Ressourcen

* [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Adobe Dokumentenerzeugung API](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [Adobe Sign-API](https://www.adobe.io/apis/documentcloud/sign.html)

* [Word-Add-in für Tagger zur Dokumentgenerierung](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=docgen-addin)

* [Projektbeispiel](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html)

## Erste Schritte

[Node.js](https://nodejs.org/) ist die Programmierplattform. Es verfügt über eine enorme Menge an Bibliotheken, wie den Express-Webserver. [Node.js herunterladen](https://nodejs.org/en/download/) und folgen Sie den Schritten, um diese großartige Open-Source-Entwicklungsumgebung zu installieren.

Um die Adobe Document Generation API in Node.js zu verwenden, navigieren Sie zu [API für Dokumentenerzeugung](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html) , um auf Ihr Konto zuzugreifen oder sich für ein neues Konto zu registrieren. Dein Konto ist [sechs Monate lang kostenlos und dann nach Bedarf bezahlen](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) für nur 0,05 US-Dollar pro Dokumenttransaktion, sodass Sie es testen können, ohne Risiken dann nur bezahlen, wenn Ihr Unternehmen wächst.

Nach der Anmeldung bei der [Adobe Developer Console](https://console.adobe.io/)klicken Sie auf **[!UICONTROL Neues Projekt erstellen]**. Das Projekt hat standardmäßig den Namen &quot;Projekt 1&quot;. Klicken Sie auf **[!UICONTROL Projekt bearbeiten]** und ändern Sie den Namen in &quot;Offer Letter Generator&quot;. In der Mitte des Bildschirms befindet sich ein **[!UICONTROL Erste Schritte mit Ihrem neuen Projekt]** Abschnitt. Um die Sicherheit Ihres Projekts zu aktivieren, führen Sie die folgenden Schritte aus:

Klicken **API hinzufügen**. Es wird eine Reihe von APIs zur Auswahl angezeigt. Im Dialogfeld **[!UICONTROL Nach Produkt filtern]** Abschnitt wählen Sie **[!UICONTROL Document Cloud]** und klicken dann auf **[!UICONTROL Weiter]**.

Generieren Sie jetzt die Anmeldeinformationen, um auf die API zuzugreifen. Die Anmeldeinformationen werden in Form eines JSON-Web-Tokens ([JWT](https://jwt.io/)): ein offener Standard für sichere Kommunikation. Wenn Sie mit JWT vertraut sind und bereits Schlüssel generiert haben, können Sie hier Ihren öffentlichen Schlüssel hochladen. Alternativ können Sie fortfahren, indem Sie **Option 1** um die Adobe zu veranlassen, die Schlüssel für Sie zu generieren.

![Screenshot des Generierens von Anmeldeinformationen](assets/offer_1.png)

Klicken Sie auf **[!UICONTROL Generieren von Schlüsselpaar]** klicken. Sie erhalten eine config.zip -Datei zum Herunterladen. Entpacken Sie die Archivdatei. Es enthält zwei Dateien: certificate_pub.crt und private.key. Achten Sie darauf, dass die Sicherheitseinstellungen aktiviert sind, da sie Ihre persönlichen Daten enthalten und dazu verwendet werden können, falsche Dokumente zu erstellen, falls Sie diese nicht mehr kontrollieren.

Klicken Sie auf **[!UICONTROL Weiter]**. Nein, ermöglicht den Zugriff auf die PDF-Generierungs-API. Im Fenster &quot; **[!UICONTROL Produktprofile auswählen]** screen, check **[!UICONTROL Enterprise PDF Services Developer]** und klicken Sie auf die Schaltfläche **[!UICONTROL Konfigurierte API speichern]** klicken. Jetzt können Sie die API verwenden.

## Einrichten des Projekts

Richten Sie ein Knotenprojekt ein, um den Code auszuführen. In diesem Beispiel wird [Visual Studio-Code](https://code.visualstudio.com/) (VS-Code) als Editor. Erstellen Sie einen Ordner namens &quot;letter-generator&quot; und öffnen Sie ihn in VS Code. Im Fenster &quot; **[!UICONTROL Datei]** wählen Sie **[!UICONTROL Terminal]** \> **[!UICONTROL Neues Terminal]** , um eine Shell in diesem Ordner zu öffnen. Überprüfen Sie, ob Node installiert ist und sich auf Ihrem Pfad befindet, indem Sie Folgendes eingeben:

```
node -v
```

Sie sollten die Version von Node sehen, die Sie installiert haben.

Nachdem Sie Ihre Entwicklungsumgebung installiert haben, können Sie mit dem Erstellen Ihres Projekts fortfahren.

Initialisieren Sie zunächst das Projekt mit dem Node Package Manager (npm). Geben Sie Folgendes ein:

```
npm init
```

Ihnen werden einige Fragen zu Ihrem Node-Projekt gestellt. Sie können die meisten dieser Fragen überspringen, aber stellen Sie sicher, dass der Projektname &quot;letter-generator&quot; ist und der Einstiegspunkt **index.js**. Auswählen **Ja** , um die Projektinitialisierung abzuschließen.

Du hast jetzt eine package.json-Datei. Node verwendet diese Datei, um Ihr Projekt zu organisieren. Bevor Sie die Datei &quot;index.js&quot; erstellen, müssen Sie Adobe-Bibliotheken mit dem folgenden Befehl hinzufügen:

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

Es sollte ein neuer Ordner mit dem Namen node_modules zu Ihrem Projekt hinzugefügt werden. In diesem Ordner werden alle Bibliotheken (im Knoten Abhängigkeiten genannt) heruntergeladen. Die Datei package.json wird ebenfalls mit einem Verweis auf Adobe PDF Services aktualisiert.

Jetzt möchten Sie Express als Ihr einfaches Web-Framework installieren. Geben Sie den folgenden Befehl ein:

```
npm install express –save
```

Wie zuvor wird der Abschnitt &quot;Abhängigkeiten&quot; von package.json entsprechend aktualisiert.

## Vorlagen für Angebotsschreiben erstellen

Erstellen Sie nun im Projektstamm eine Datei mit dem Namen &quot;app.js&quot;. Fügen wir folgenden Startcode hinzu:

```
const express = require('express');
const bodyParser = require('body-parser');
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk')
const path = require('path');
const app = express();
const port = 8000;
app.use(bodyParser.urlencoded({ extended: true }));
app.get('/', (req, res) => {
res.sendFile(path.join(__dirname + '/index.html'));
});
app.post('/', (req, res) => {
console.log('Got body:', req.body);
res.sendStatus(200);
});
app.listen(port, () => {
console.log(`Candidate offer letter app listening on port ${port}!`)
});
```

Beachten Sie, dass die GET-Route eine **index.html** -Datei. Erstellen Sie eine HTML-Datei mit diesem Namen und der folgenden einfachen Form. Sie können CSS-Stile und andere Design-Elemente später nach Bedarf hinzufügen. Dieses Formular enthält die grundlegenden Details des Bewerbers zum Erstellen eines Begrüßungsschreibens:

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Offer Letter Generator</title>
</head>
<body>
<h1>Enter Candidate Details</h1>
<form action="" method="post">
<div>
<label for="firstname">First name: </label>
<input type="text" name="firstname" id="firstname" required>
</div>
<div>
<label for="lastname">Last name: </label>
<input type="text" name="lastname" id="lastname" required>
</div>
<div>
<label for="salary">Salary ($): </label>
<input type="number" name="salary" id="salary" required>
</div>
<div>
<label for="startdate">Start Date: </label>
<input type="date" name="startdate" id="startdate" required>
</div>
<div>
<input type="submit" value="Generate Letter">
</div>
</form>
</body>
</html>
```

Führen Sie den Webserver mit dem folgenden Befehl aus:

```
node app.js
```

Sie sollten die Meldung &quot;Candidate offer letter app listen on port 8000&quot; sehen. Wenn Sie Ihren Browser öffnen, um <http://localhost:8000/>sollte das Formular wie folgt aussehen:

![Screenshot des Webformulars](assets/offer_2.png)

Beachten Sie, dass das Formular an sich selbst gesendet wird. Wenn Sie Daten eingeben und auf **Buchstaben generieren** Sie sollten die folgenden Informationen auf der Konsole sehen:

```
Got body: { firstname: 'John',
lastname: 'Doe',
salary: '887888',
startdate: '2021-04-01' }
```

Sie ersetzen diese Konsolenprotokollierung durch einen Webdienstaufruf an [!DNL Acrobat Services]. Zunächst müssen Sie ein JSON-basiertes Modell der Informationen erstellen. Das Format dieses Modells sieht folgendermaßen aus:

```
{
    "offer_letter": {
    "firstname": "John",
    "lastname": "Doe",
    "salary": "887888",
    "startdate": "2021-04-01"
    }
}
```

Du kannst das Modell auf Wunsch etwas aufwändiger gestalten. In diesem Tutorial lernst du aber ein einfaches Beispiel. Dieses Formular enthält keine Validierung, da es nicht in den Geltungsbereich dieses Artikels fällt. Um den Formulartext in das oben beschriebene Datenmodell zu konvertieren, ändern Sie die app.post Handler-Methode in folgenden Code:

```
app.post('/', (req, res) => {
const docModel = {'offer_letter': req.body};
generateLetter(docModel);
res.sendStatus(200);
});
```

Die erste Zeile platziert Ihre JSON-Daten im gewünschten Format. Diese Daten übergeben Sie nun an eine generateLetter-Funktion. Beenden Sie den Server und fügen Sie den folgenden Code am Ende von app.js ein. In diesem Code wird ein Word-Dokument als Vorlage verwendet und Platzhalter werden mit Informationen aus einem JSON-Dokument gefüllt.

```
// Letter generation function
function generateLetter(jsonDataForMerge) {
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Create an ExecutionContext using credentials
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
// Create a new DocumentMerge options instance
const documentMerge = PDFToolsSdk.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge,
documentMergeOptions.OutputFormat.PDF);
// Create a new operation instance using the options instance
const documentMergeOperation = documentMerge.Operation.createNew(options)
// Set operation input document template from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile(
'resources/OfferLetter-Template.docx');
documentMergeOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
.then(result => result.saveAsFile('output/OfferLetter.pdf'))
.catch(err => {
if(err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log(
'Exception encountered while executing operation', err);
} else {
console.log(
'Exception encountered while executing operation', err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

Dort gibt es viel Code zum Entpacken. Lassen Sie uns zuerst den Hauptteil betrachten: der `documentMergeOperation`. In diesem Abschnitt können Sie Ihre JSON-Daten mit einer Word-Dokumentvorlage zusammenführen. Sie können den Befehl [Beispiel auf der Website der Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html#sample-blade) als Referenz, aber lassen Sie uns Ihr eigenes, einfaches Beispiel erstellen. Öffnen Sie Word und erstellen Sie ein neues leeres Dokument. Du kannst sie beliebig oft anpassen, hast aber wenigstens etwas Ähnliches:

Hallo X,

Wir freuen uns, Ihnen eine Position für $X pro Jahr anbieten zu können. Ihr Startdatum ist X.

Willkommen

Speichern Sie das Dokument unter dem Namen &quot;OfferLetter-Template.docx&quot; im Ordner &quot;resources&quot; im Stammordner Ihres Projekts. Beachten Sie die drei Xs in dem Dokument. Diese Xs sind temporäre Platzhalter für Ihre JSON-Informationen. Obwohl Sie diese Platzhalter durch eine spezielle Syntax ersetzen können, bietet Adobe ein Word-Add-in, das diese Aufgabe vereinfacht. Um das Add-in zu installieren, gehen Sie zur Adobe [Word-Add-in für Tagger zur Dokumentgenerierung](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=docgen-addin) Website.

Klicken Sie in Ihrer OfferLetter-Vorlage auf das neue **Dokumenterstellung** klicken. Ein Seitenbedienfeld wird geöffnet. Klicken Sie auf **Erste Schritte**. Es wird ein Textbereich zum Einfügen in die JSON-Beispieldaten bereitgestellt. Kopieren Sie das &quot;Angebot-Daten&quot;-Snippet von JSON von oben in den Textbereich. Es sollte wie folgt aussehen:

![Screenshot von Buchstaben und Code](assets/offer_3.png)

Klicken Sie auf **Tags generieren** klicken. Sie erhalten ein Dropdown-Menü mit Tags, die Sie an den entsprechenden Stellen im Dokument einfügen können. Markieren Sie das erste X im Dokument und wählen Sie **[!UICONTROL Vorname]**. Klicken **[!UICONTROL Text einfügen]** und &quot;Lieber X,&quot; wird in &quot;Lieber X, ```{{`offer_letter`.firstname}}```,&quot;. Dieses Tag hat das richtige Format für `documentMergeOperation`. Nun fügen Sie die restlichen drei Tags an den entsprechenden Xs hinzu. Vergessen Sie nicht, OfferLetter-template.docx zu speichern. Es sollte wie folgt aussehen:

Sehr geehrte*r ```{{`offer_letter`.firstname}} {{`offer_letter`.lastname}}```,

Wir freuen uns, Ihnen eine Position für $ ```{{`offer_letter`.salary}}``` pro Jahr. Dein Startdatum ist der . ```{{`offer_letter`.startdate}}```.

Willkommen

Die Word-Vorlage enthält Markup, das dem JSON-Format entspricht. Beispiel: ```{{`offer_letter`.`firstname`}}``` am Anfang des Word-Dokuments wird durch den Wert im Abschnitt &quot;Vorname&quot; der JSON-Daten ersetzt.

Zurück zu Ihrem `generateLetter` Funktion. Erstellen Sie zum Sichern Ihres REST-Aufrufs eine neue Datei mit dem Namen pdftools-api-credentials.json im Projektstamm. Fügen Sie die folgenden JSON-Daten ein und passen Sie sie mit Details aus dem Abschnitt Service Account (JWT) Ihres [Entwicklerkonsole](https://console.adobe.io/).

```
{
"client_credentials": {
"client_id": "<YOUR_CLIENT_ID>",
"client_secret": "<YOUR_CLIENT_SECRET>"
},
"service_account_credentials": {
"organization_id": "<YOUR_ORGANIZATION_ID>",
"account_id": "<YOUR_TECHNICAL_ACCOUNT_ID>",
"private_key_file": "<PRIVATE_KEY_FILE_PATH>"
}
}
```

* Die Client-ID, der Client-Schlüssel und die Organisations-ID können direkt aus der **[!UICONTROL Berechtigungsdetails]** der Konsole.

* Die Konto-ID ist die **Technische Konto-ID**.

* Kopieren Sie die zuvor generierte Datei private.key in das Projekt und geben Sie ihren Namen im Abschnitt private_key_file der Datei pdftools-api-credentials.json ein. Wenn Sie möchten, können Sie hier einen Pfad zur privaten Schlüsseldatei angeben. Denken Sie daran, es sicher zu halten, da es falsch verwendet werden kann, sobald Sie es nicht mehr kontrollieren.

Um eine PDF mit den ausgefüllten JSON-Daten zu generieren, kehren Sie zu Ihrem **[!UICONTROL Bewerberdetails eingeben]** Webformular aus und senden Sie einige Daten. Es dauert etwas, da das Dokument von Adobe heruntergeladen werden muss, aber Sie sollten eine Datei mit dem Namen OfferLetter.pdf in einem neuen Ordner mit dem Titel &quot;output&quot; haben.

## Nächste Schritte

Das ist alles! Das ist erst der Anfang. Wenn Sie sich im Abschnitt &quot;Erweitert&quot; der Registerkarte &quot;Dokumenterstellung&quot; des Word-Add-ins umsehen, werden Sie feststellen, dass nicht alle Platzhaltermarken aus den zugeordneten JSON-Daten stammen. Sie können auch Signatur-Tags hinzufügen. Mit diesen Tags können Sie das resultierende Dokument in [Adobe Sign](https://acrobat.adobe.com/ca/en/sign.html) zur Zustellung und Unterzeichnung an den neuen Mitarbeiter. Lesen Sie &quot;Erste Schritte mit der Adobe Sign API&quot;, um mehr über diese Möglichkeiten zu erfahren. Dieser Vorgang ist ähnlich, da Sie REST-Aufrufe verwenden, die mit einem JWT-Token gesichert sind.

Das oben angegebene Beispiel für ein einzelnes Dokument kann als Grundlage für eine Anwendung verwendet werden, wenn eine Organisation [Beschleunigung der Saisonbeschäftigung](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html) von Mitarbeitern an mehreren Standorten. Wie gezeigt, besteht der Hauptfluss darin, Daten von Bewerbern über eine Online-Bewerbung zu sammeln. Die Daten werden verwendet, um die Felder eines Angebotsschreibens auszufüllen und zum elektronischen Unterschreiben zu senden.

[!DNL Adobe Acrobat Services] für sechs Monate kostenlos ist, dann [pay as you go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) mit nur 0,05 US-Dollar pro Dokumenttransaktion, sodass Sie sie ausprobieren und Ihren Workflow für Angebotsschreiben skalieren können, wenn Ihr Unternehmen wächst. Funktion [Erste Schritte](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
Erstellen eigener Vorlagen [Registrieren Sie sich bei Ihrem Entwicklerkonto](https://www.adobe.io/).
