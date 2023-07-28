---
title: Suchen und Indizieren
description: Erfahren Sie, wie Sie aus gescannten Dokumenten durchsuchbare PDF-Dateien erstellen.
role: Developer
level: Intermediate
type: Tutorial
feature: Use Cases
thumbnail: KT-8095.jpg
jira: KT-8095
exl-id: a22230b5-1ff2-4870-84da-f06a904c99e1
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '1364'
ht-degree: 1%

---

# Suchen und Indizieren

![Use Case Hero-Banner](assets/UseCaseSearchingHero.jpg)

Unternehmen müssen oft ihre Papierdokumente und gescannten Dateien digitalisieren. Folgendes berücksichtigen: [Szenario](https://docs.google.com/document/d/11jZdVQAw-3fyE3Y-sIqFFTlZ4m02LsCC/edit). Eine Anwaltskanzlei hat Tausende von Verträgen, die sie gescannt haben, um digitale Dateien zu erstellen. Sie wollen feststellen, ob in einem dieser Verträge eine bestimmte Klausel oder ein bestimmter Zusatz enthalten ist, den sie überarbeiten müssen. Für Compliance-Zwecke ist Präzision erforderlich. Die Lösung besteht darin, eine Bestandsaufnahme der digitalen Dokumente vorzunehmen, den Text durchsuchbar zu machen und einen Index zu erstellen, um diese Informationen zu finden.

Für die meisten Organisationen ist die Erstellung digitaler Archive zum Abrufen von Informationen für die Bearbeitung oder den Downstream-Betrieb ein Albtraum.

## Lernziel.

In diesem Tutorial erfahrt ihr, [!DNL Adobe Acrobat Services] Die Funktionen von APIs und können einfach zur Archivierung und Digitalisierung von Dokumenten verwendet werden. Sie erkunden diese Funktionen, indem Sie eine Express NodeJS-Anwendung erstellen und dann [!DNL Acrobat Services] APIs für Archivierung, Digitalisierung und Transformation von Dokumenten.

Um zu folgen, benötigen Sie [Node.js](https://nodejs.org/) installiert und grundlegende Kenntnisse über Node.js und [ES6-Syntax](https://www.w3schools.com/js/js_es6.asp).

## Relevante APIs und Ressourcen

* [PDF Services-API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Projektcode](https://github.com/agavitalis/AdobeDocumentServicesAPIs.git)

## Projekteinrichtung

Richten Sie zunächst die Ordnerstruktur für die Anwendung ein. Sie können den Quellcode abrufen [hier](https://github.com/agavitalis/AdobeDocumentAPI.git).

## Verzeichnisstruktur

Erstellen Sie einen Ordner mit dem Namen AdobeDocumentServicesAPIs und öffnen Sie ihn in einem Editor Ihrer Wahl. Erstellen Sie eine grundlegende NodeJS-Anwendung mit dem Katalog `npm init` Befehl mit dieser Ordnerstruktur:

```
AdobeDocumentServicesAPIs
config
default.json
controllers
createPDFController.js
makeOCRController.js
searchController.js
models
document.js
output
.gitkeep
routes
web.js
services
upload.js
views
index.hbs
ocr.hbs
search.hbs
index.js
```

Sie verwenden MongoDB als Datenbank für diese Anwendung. Zum Konfigurieren platzieren Sie daher Ihre Standarddatenbankkonfigurationen im Ordner &quot;config/&quot;, indem Sie den unten stehenden Codeausschnitt in die Datei &quot;default.json&quot; dieses Ordners einfügen und dann die URL Ihrer Datenbank hinzufügen.

```
### config/default.json and config/dev.json
{ "DBHost": "YOUR_DB_URI" }
```

## Paketinstallation

Installieren Sie jetzt einige Pakete mit dem Befehl npm install, wie im folgenden Codefragment gezeigt:

```
{
    "name": "adobedocumentservicesapis",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "directories": {
    "test": "test"
    },
    "dependencies": {
    "body-parser": "^1.19.0",
    "config": "^3.3.6",
    "express": "^4.17.1",
    "hbs": "^4.1.1",
    "mongoose": "^5.12.1",
    "morgan": "^1.10.0",
    "multer": "^1.4.2",
    "path": "^0.12.7"
    },
    "devDependencies": {},
    "scripts": {
    "start": "set NODE_ENV=dev && node index.js"
    },
    "repository": {
    "type": "git",
    "url": "git+https://github.com/agavitalis/AdobeDocumentServicesAPIs.git"
    },
    "author": "Ogbonna Vitalis",
    "license": "ISC",
    "bugs": {
    "url": "https://github.com/agavitalis/AdobeDocumentServicesAPIs/issues"
    },
    "homepage": "https://github.com/agavitalis/AdobeDocumentServicesAPIs#readme"
}
```

```
###bash
npm install express mongoose config body-parser morgan multer hbs path pdf-parse
Ensure that the content of your package.json file is similar to this code snippet:
###package.json
{
```

Mit diesen Codeausschnitten werden die Anwendungsabhängigkeiten installiert, einschließlich des Handlebars-Vorlagenmoduls für die Ansicht. Im script-Tag konfigurieren Sie die Laufzeitparameter der Anwendung.

## Integrieren [!DNL Acrobat Services] API

[!DNL Acrobat Services] umfasst drei APIs:

* Adobe PDF Services API

* Adobe PDF Embed-API

* API für die Dokumentenerzeugung in Adoben

Diese APIs automatisieren die Erstellung, Bearbeitung und Transformation von PDF-Inhalten über eine Reihe Cloud-basierter Webdienste.

Zum Abrufen der erforderlichen Anmeldeinformationen müssen Sie [registrieren](https://www.adobe.com/go/dcsdks_credentials?ref=getStartedWithServicesSDK) und schließen Sie den Arbeitsablauf ab. Die PDF Embed-API kann kostenlos verwendet werden. PDF Services API und Document Generation API sind sechs Monate lang kostenlos. Wenn Ihr Probe-Abo endet, können Sie [pay as you go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) bei nur 0,05 US-Dollar pro Dokumenttransaktion. Sie zahlen nur, wenn Ihr Unternehmen wächst und mehr Verträge verarbeitet.

![Screenshot der Erstellung von Anmeldedaten](assets/searching_1.png)

Nachdem Sie die Registrierung abgeschlossen haben, wird ein Codebeispiel auf Ihren PC heruntergeladen, das Ihre API-Zugangsberechtigungen enthält. Extrahieren Sie dieses Codebeispiel, und platzieren Sie die Dateien &quot;private.key&quot; und &quot;pdftools-api-credentials.json&quot; im Stammverzeichnis der Anwendung.

Jetzt installieren Sie [PDF Services Node.js SDK](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk) , indem Sie die ` npm install --save @adobe/documentservices-pdftools-node-sdk ` mit dem Terminal im Stammverzeichnis der Anwendung.

## Erstellen einer PDF

[!DNL Acrobat Services] unterstützt die Erstellung von PDF aus Microsoft Office-Dokumenten (Word, Excel und PowerPoint) und anderen [Unterstützte Dateiformate](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf) wie .txt, .rtf, .bmp, .jpg, .gif, .tiff und .png.

Um PDF-Dokumente aus den unterstützten Dateiformaten zu erstellen, verwenden Sie dieses Formular, um die Dokumente hochzuladen. Sie können auf die HTML- und CSS-Dateien für das Formular zugreifen, indem Sie auf der [GitHub](https://github.com/agavitalis/AdobeDocumentServicesAPIs.git).

![Screenshot des Webformulars](assets/searching_2.png)

Fügen Sie nun die folgenden Codefragmente zur Datei controllers/createPDFController.js hinzu. Dieser Code ruft das Dokument ab und wandelt es in eine PDF um.

Die Originaldateien und die transformierte Datei werden in einem Ordner innerhalb der Anwendung gespeichert.

```
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
/*
* GET / route to show the createPDF form.
*/
function createPDF(req, res) {
//catch any response on the url
let response = req.query.response
res.render('index', { response })
}
/*
* POST /createPDF to create a new PDF File.
*/
function createPDFPost(req, res) {
let filePath = req.file.path;
let fileName = req.file.filename;
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Create an ExecutionContext using credentials and create a new operation
instance.
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials),
createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();
// Set operation input from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile(filePath);
createPdfOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
createPdfOperation.execute(executionContext)
.then((result) => {
result.saveAsFile('output/createPDFFromDOCX.pdf')
//download the file
res.redirect('/?response=PDF Successfully created')
})
.catch(err => {
if (err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log('Exception encountered while executing operation',
err);
} else {
console.log('Exception encountered while executing operation',
err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
//export all the functions
module.exports = { createPDF, createPDFPost };
```

Dieses Codefragment erfordert [PDF Services Node.js SDK](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk). Verwenden Sie die Funktionen:

* createPDF mit dem Formular zum Hochladen des Dokuments

* createPDFPost, der das hochgeladene Dokument auf eine PDF transformiert

Die transformierten PDF-Dokumente werden im Ausgabeverzeichnis gespeichert, während die Originaldatei im Ordner &quot;uploads&quot; gespeichert wird.

## Verwenden der Texterkennung

Die optische Zeichenerkennung (OCR) konvertiert Bilder und gescannte Dokumente in durchsuchbare Dateien. Sie können [!DNL Acrobat Services] APIs, Bilder und gescannte Dokumente in durchsuchbare PDF umwandeln Nach einem OCR-Vorgang kann die Datei bearbeitet und durchsucht werden. Sie können den Inhalt der Datei in einem Datenspeicher für die Indizierung und andere Zwecke speichern.

Denken Sie daran, dass das Suchen und Indizieren gescannter Dokumente für viele Unternehmen, in denen Dateiverwaltung und Informationsverarbeitung unverzichtbar sind, von entscheidender Bedeutung ist. Mit der OCR-Funktion werden diese Herausforderungen behoben.

Um diese Funktion zu implementieren, müssen Sie ein Upload-Formular ähnlich dem oben genannten entwerfen. Dieses Mal beschränken Sie das Formular auf das PDF von Dateien, da Sie die OCR-Funktion nur auf dem PDF von Dokumenten verwenden können.

Hier ist das Upload-Formular für dieses Beispiel:

![Screenshot des Formulars zum Hochladen von Dateien](assets/searching_3.png)

Wenn Sie nun die hochgeladene PDF bearbeiten und einige OCR-Vorgänge ausführen möchten, fügen Sie den folgenden Codeausschnitt zur Datei controllers/makeOCRController.js hinzu. Dieser Code implementiert den OCR-Prozess für eine hochgeladene Datei und speichert dann die Datei im Dateisystem der Anwendung.

```
const fs = require('fs')
const pdf = require('pdf-parse');
const mongoose = require('mongoose');
const Document = require('../models/document');
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
/*
* GET /makeOCR route to show the makeOCR form.
*/
function makeOCR(req, res) {
//catch any response on the url
let response = req.query.response
res.render('ocr', { response })
}
/*
* POST /makeOCRPost to create a new PDF File.
*/
function makeOCRPost(req, res) {
let filePath = req.file.path;
let fileName = req.file.filename;
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Create an ExecutionContext using credentials and create a new operation
instance.
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials),
ocrOperation = PDFToolsSdk.OCR.Operation.createNew();
// Set operation input from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile(filePath);
ocrOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
ocrOperation.execute(executionContext)
.then(async (result) => {
let newFileName = `createPDFFromDOCX-${Math.random() * 171}.pdf`;
await result.saveAsFile(`output/${newFileName}`);
let documentContent = fs.readFileSync(
require("path").resolve("./") + `\\output\\${newFileName}`
);
pdf(documentContent)
.then(function (data) {
//Creates a new document
var newDocument = new Document({
documentName: fileName,
documentDescription: description,
documentContent: data.text,
url: require("path").resolve("./") + `\\output\\${newFileName}`
});
//Save it into the DB.
newDocument.save((err, docs) => {
if (err) {
res.send(err);
} else {
//If no errors, send it back to the client
res.redirect(
"/makeOCR?response=OCR Operation Successfully performed on
the PDF File"
);
}
});
})
.catch(function (error) {
// handle exceptions
console.log(error);
});
})
.catch(err => {
if (err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log('Exception encountered while executing operation',
err);
} else {
console.log('Exception encountered while executing operation',
err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
//export all the functions
module.exports = { makeOCR, makeOCRPost };
```

Sie benötigen die [!DNL Acrobat Services] Knoten-SDK und die Mongoose-, PDF-Parse- und FS-Module sowie Ihr Dokumentmodellschema. Diese Module sind notwendig, um den Inhalt der transformierten Datei in einer MongoDB-Datenbank zu speichern.

Erstellen Sie nun zwei Funktionen: makeOCR zum Anzeigen des hochgeladenen Formulars und makeOCRPost zum Verarbeiten des hochgeladenen Dokuments. Speichern Sie das ursprüngliche Formular in einer Datenbank und speichern Sie das transformierte Formular dann im Ausgabeordner Ihrer Anwendung.

Die von der Adobe bereitgestellten Anmeldeinformationen aus der Datei pdftools-api-credentials.json werden jeweils vor dem Transformieren der Datei geladen.

>[!NOTE]
>
>Die OCR-Funktion unterstützt nur das PDF von Dokumenten.

Fügen Sie außerdem den folgenden Codeausschnitt zur Datei Modes/Document.js Ihrer Anwendung hinzu.

Definieren Sie im Codefragment ein Mungosemodell und beschreiben Sie dann die Eigenschaften des Dokuments, die in der Datenbank gespeichert werden sollen. Indizieren Sie außerdem das Feld documentContent, um die Suche nach Texten einfach und effizient zu gestalten.

```
const mongoose = require("mongoose");
const Schema = mongoose.Schema;
//Document schema definition
var DocumentSchema = new Schema(
{
documentName: { type: String, required: false },
documentDescription: { type: String, required: false },
documentContent: { type: String, required: false },
url: { type: String, required: false },
status: {
type: String,
enum : ["active","inactive"],
default: "active"
}
},
{ timestamps: true }
);
//for text search
DocumentSchema.index({
documentContent: "text",
});
//Exports the DocumentSchema for use elsewhere.
module.exports = mongoose.model("document", DocumentSchema);
```

## Suchen von Texten

Jetzt implementieren Sie eine einfache Suchfunktion, mit der Benutzer einige einfache Textsuchen durchführen können. Sie können auch Downloadfunktionen hinzufügen, um das Herunterladen von PDF-Dateien zu aktivieren.

Diese Funktion erfordert ein einfaches Formular und Karten, um das Suchergebnis anzuzeigen. Sie finden die Designs für das Formular und die Karten unter [GitHub](https://github.com/agavitalis/AdobeDocumentServicesAPIs.git).

Der folgende Screenshot zeigt die Suchfunktion und die Suchergebnisse. Sie können alle Suchergebnisse herunterladen.

![Screenshot der Suchfunktionen](assets/searching_4.png)

Um die Suchfunktion zu implementieren, erstellen Sie die Datei searchController.js im Ordner des Controllers Ihrer Anwendung und fügen Sie den folgenden Codeausschnitt ein:

```
const fs = require('fs')
const mongoose = require('mongoose');
const Document = require('../models/document');
/*
* GET / route to show the search form.
*/
function search(req, res) {
//catch any response on the url
let response = req.query.response
res.render('search', { response })
}
/*
* POST /searchPost to search the contents of your saved file.
*/
function searchPost(req, res) {
let searchString = req.body.searchString;
Document.aggregate([
{ $match: { $text: { $search: searchString } } },
{ $sort: { score: { $meta: "textScore" } } },
])
.then(function (documents) {
res.render('search', { documents })
})
.catch(function (error) {
let response = error
res.render('search', { response })
});
}
//export all the functions
module.exports = { search, searchPost, downloadPDF };
```

Implementieren Sie jetzt eine Download-Funktion, mit der Sie das Herunterladen der Dokumente aktivieren können, die von der Suche eines Benutzers zurückgegeben werden.

## Dokumente herunterladen

Die Implementierung einer Download-Funktion ähnelt dem, was Sie bereits getan haben. Fügen Sie den folgenden Codeausschnitt nach der searchPost -Funktion in der Datei controllers/earchController.js hinzu:

```
/*
* POST /downloadPDF To Download PDF Documents.
*/
async function downloadPDF(req, res) {
console.log("here")
let documentId = req.params.documentId
let document = await Document.findOne({_id:documentId});
res.download(download.link);
}
```

## Nächste Schritte

In diesem praktischen Tutorial haben Sie Folgendes integriert: [!DNL Acrobat Services] APIs in eine Node.js-Anwendung konvertiert und die API auch verwendet, um eine Dokumenttransformation zu implementieren, die Dateien in PDF konvertiert. Sie haben eine OCR-Funktion hinzugefügt, mit der Bilder und gescannte Dateien durchsuchbar werden. Anschließend haben Sie die Dateien in einem Ordner gespeichert, sodass sie heruntergeladen werden können.

Als Nächstes haben Sie eine Suchfunktion hinzugefügt, um die Dokumente zu durchsuchen, die durch OCR in Text konvertiert wurden. Schließlich haben Sie eine Download-Funktion implementiert, um das einfache Herunterladen dieser Dateien zu ermöglichen. Mit Ihrer ausgefüllten Bewerbung können Rechtsanwälte bestimmten Text viel einfacher finden und bearbeiten.

Verwenden [!DNL Acrobat Services] für die Transformation von Dokumenten wird aufgrund ihrer Robustheit und Benutzerfreundlichkeit im Vergleich zu anderen Diensten dringend empfohlen. Sie können schnell ein Konto erstellen, um die Funktionen von [!DNL Acrobat Services] APIs für die Transformation und Verwaltung von Dokumenten.

Sie wissen nun, wie Sie [!DNL Acrobat Services] Mit APIs kannst du deine Kenntnisse mit Übungen vertiefen. Sie können das Repository klonen, das in diesem Tutorial verwendet wird, und mit einigen der Kenntnisse experimentieren, die Sie gerade gelernt haben. Noch besser ist, dass Sie versuchen können, diese Anwendung neu zu erstellen, während Sie die unbegrenzten Möglichkeiten der [!DNL Acrobat Services] APIs

Sind Sie bereit, die Freigabe von Dokumenten und die Überprüfung in Ihrer eigenen App zu aktivieren? Registrieren für [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
Entwicklerkonto. Teste das Programm sechs Monate lang kostenlos. Danach [pay as you go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) für nur \$0.05 pro Dokumenttransaktion, wenn Ihr Unternehmen wächst.
