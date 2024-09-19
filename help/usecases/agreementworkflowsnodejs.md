---
title: Vereinbarungsarbeitsabläufe in Node.js
description: "[!DNL Adobe Acrobat Services] APIs integrieren PDF-Funktionen ganz einfach in Ihre Webanwendungen."
feature: Use Cases
role: Developer
level: Beginner
type: Tutorial
jira: KT-7473
thumbnail: KT-7473.jpg
keywords: Empfohlen
exl-id: 44a03420-e963-472b-aeb8-290422c8d767
source-git-commit: f8a31b8f98d99bf1f3787e0f0f19cc9f26e24d8d
workflow-type: tm+mt
source-wordcount: '2094'
ht-degree: 0%

---

# Vereinbarungs-Workflows in Node.js

![Banner &quot;Use Case Hero&quot;](assets/UseCaseAgreementHero.jpg)

Viele Geschäftsanwendungen und -prozesse erfordern Dokumentationen wie Angebote und Vereinbarungen. Das PDF von Dokumenten stellt sicher, dass Dateien sicherer und weniger veränderbar sind. Zusätzlich werden digitale Signaturen unterstützt, sodass Kunden ihre Dokumente schnell und einfach ausfüllen können. [!DNL Adobe Acrobat Services] APIs integrieren PDF-Funktionen ganz einfach in Ihre Webanwendungen.

## Lernziel.

In diesem Tutorial lernen Sie, wie Sie PDF-Services zu einer Anwendung &quot;Node.js&quot; hinzufügen, um einen Vereinbarungsprozess zu digitalisieren.

## Relevante APIs und Ressourcen

* [PDF Services-API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF Embed-API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Adobe Sign-API](https://www.adobe.io/apis/documentcloud/sign.html)

* [Projektcode](https://github.com/adobe/pdftools-node-sdk-samples)

## [!DNL Adobe Acrobat Services] wird eingerichtet.

Richten Sie zunächst Anmeldeinformationen ein, um [!DNL Adobe Acrobat Services] zu verwenden. Registrieren Sie ein Konto, und überprüfen Sie mit [Node.js Quickstart](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#node-js), ob Ihre Anmeldeinformationen funktionieren, bevor Sie die Funktionen in eine größere Anwendung integrieren.

Besorgen Sie sich zunächst ein Adobe-Entwicklerkonto. Wählen Sie dann auf der Seite [Erste Schritte](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK) die Option *Erste Schritte* unter Neue Anmeldeinformationen erstellen aus. Sie können sich für die kostenlose Testversion anmelden, die 1.000 Dokumenttransaktionen bietet, die über sechs Monate verwendet werden können.

![Image der Erstellung neuer Anmeldeinformationen](assets/AWNjs_1.png)

Auf der folgenden Seite Neue Anmeldeinformationen erstellen werden Sie aufgefordert, zwischen der PDF Embed-API und der PDF Services-API zu entscheiden.

Wählen Sie *PDF Services API*.

Geben Sie einen Namen für die Anwendung ein und aktivieren Sie das Kontrollkästchen *Personalisiertes Codebeispiel erstellen*. Wenn Sie dieses Kontrollkästchen aktivieren, werden Ihre Anmeldeinformationen automatisch in das Codebeispiel eingebettet. Wenn Sie dieses Kontrollkästchen deaktiviert lassen, müssen Sie der Anwendung manuell Ihre Anmeldeinformationen hinzufügen.

Wählen Sie *Node.js* als Anwendungstyp aus und klicken Sie auf *Anmeldeinformationen erstellen*.

Einige Augenblicke später wird eine ZIP-Datei mit einem Beispielprojekt einschließlich Ihrer Anmeldedaten heruntergeladen. Das Paket &quot;Node.js&quot; für &quot;[!DNL Acrobat Services]&quot; ist bereits als Teil des Beispielprojektcodes enthalten.

![Image der Auswahl von PDF Services-API-Zugangsberechtigungen](assets/AWNjs_2.png)

## Manuelles Konfigurieren des Beispielprojekts

Wenn Sie kein Beispielprojekt von der Seite &quot;Neue Anmeldeinformationen erstellen&quot; herunterladen, können Sie das Projekt auch manuell einrichten.

Laden Sie den Code (ohne Ihre Anmeldeinformationen) von [GitHub](https://github.com/adobe/pdftools-node-sdk-samples) herunter. Wenn Sie diese Version des Codes verwenden, müssen Sie Ihre Anmeldeinformationen vor der Verwendung der Datei pdftools-api-credentials.json hinzufügen:

```
{
  "client_credentials": {
    "client_id": "<client_id>",
    "client_secret": "<client_secret>"
  },
  "service_account_credentials": {
    "organization_id": "<organization_id>",
    "account_id": "<technical_account_id>",
    "private_key_file": "<private_key_file_path>"
  }
}
```

Für Ihre eigene Anwendung müssen Sie die private Schlüsseldatei und die Anmeldeinformationsdateien in die Anwendungsquelle kopieren.

Sie müssen das Paket &quot;Node.js&quot; für [!DNL Acrobat Services] installieren. Verwenden Sie den folgenden Befehl, um das Paket zu installieren:

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

## Einrichten der Protokollierung

Die Beispiele hier verwenden Express für das Anwendungs-Framework. Sie verwenden log4js auch für die Anwendungsprotokollierung. Mit log4js können Sie die Protokollierung an die Konsole oder in eine Datei einfach umleiten:

```
const log4js = require('log4js');
const logger = log4js.getLogger();
log4js.configure( {
    appenders: { fileAppender: { type:'file', filename: './logs/applicationlog.txt'}},
    categories: { default: {appenders: ['fileAppender'], level:'info'}}
});
 
logger.level = 'info';
logger.info('Application started')
```

Der obige Code schreibt die protokollierten Daten in eine Datei in ./logs/applicationlog.txt Wenn Sie stattdessen in die Konsole schreiben möchten, können Sie den Aufruf an log4js.configure auskommentieren.

## Word-Dokumente in PDF-Dateien umwandeln

Verträge und Angebote werden oft in einer Produktivitäts-Anwendung wie Microsoft Word verfasst. Um Dokumente in diesem Format zu akzeptieren und das Dokument in das PDF-Format zu konvertieren, können Sie Funktionen für Ihre Anwendung hinzufügen. Sehen wir uns an, wie ein Dokument in einer Express-Anwendung hochgeladen und gespeichert und in einem Dateisystem gespeichert wird.

Fügen Sie auf der HTML der Anwendung ein Dateielement und eine Schaltfläche zum Starten des Uploads hinzu:

```
<input type="file" name="source" id="source" />
<button onclick="upload()" >Upload</button>
```

Laden Sie die Datei im JavaScript der Seite asynchron mit der Funktion fetch hoch:

```
function upload()
{
  let formData = new FormData();
  var selectedFile = document.getElementById('source').files[0];
  formData.append("source", selectedFile);
  fetch('documentUpload', {method:"POST", body:formData});
}
```

Wählen Sie einen Ordner aus, in den die hochgeladenen Dateien übernommen werden sollen. Die Anwendung benötigt einen Pfad zu diesem Ordner. Suchen Sie den absoluten Pfad, indem Sie einen relativen Pfad verwenden, der mit \_\_dirname verknüpft ist:

```
const uploadFolder = path.join(__dirname, "../uploads");
```

Da die Datei per Post übermittelt wird, müssen Sie auf eine Nachricht auf der Server-Seite antworten:

```
router.post('/', (req, res, next) => {
  console.log('uploading')
  if(!req.files || Object.keys(req.files).length === 0) {
  return res.status(400).send('No files were uploaded');
  }
    
  const uploadPath = path.join(uploadFolder, req.files.source.name);
  var buffer = req.files.source.data;
  var result = {"success":true};
  fs.writeFile(uploadPath, buffer, 'binary', (err)=> {
    if(err) {
      result.success = false;
    }
    res.json(result);
  });       
});
```

Nach Ausführung dieser Funktion wird die Datei im Upload-Ordner der Anwendungen gespeichert und steht zur weiteren Verarbeitung zur Verfügung.

Konvertieren Sie als Nächstes die Datei vom nativen Format in das PDF-Format. Der Beispielcode, den Sie zuvor heruntergeladen haben, enthält ein Skript mit dem Namen &quot;`create-pdf-from-docx.js`&quot; zum Konvertieren eines Dokuments in einen PDF. Die folgende Funktion, `convertDocumentToPDF`, nimmt ein hochgeladenes Dokument und konvertiert es in eine PDF in einem anderen Ordner:

```
function convertDocumentToPDF(sourcePath, destinationPath)
{    
  try {   
    const credentials = PDFToolsSDK.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdftools-api-credentials.json")
    .build();
 
    const executionContext = 
      PDFToolsSDK.ExecutionContext.create(credentials),
    createPdfOperation = PDFToolsSDK.CreatePDF.Operation.createNew();
 
    const docxReadableStream = fs.createReadStream(sourcePath);
    const input = PDFToolsSDK.FileRef.createFromStream(
      docxReadableStream, 
      PDFToolsSDK.CreatePDF.SupportedSourceFormat.docx);
    createPdfOperation.setInput(input);
 
    createPdfOperation.execute(executionContext)
    .then(result => result.saveAsFile(destinationPath))
    .catch(err => {        
      logger.erorr('Exception encountered while executing operation');        
    })
  }
  catch(err) {        
    logger.error(err);
  }
}
```

Möglicherweise fällt Ihnen ein allgemeines Muster mit dem Code auf:

Der Code erstellt ein Anmeldeinformationsobjekt und einen Ausführungskontext, initialisiert einen Vorgang und führt dann den Vorgang mit dem Ausführungskontext aus. Sie können dieses Muster im gesamten Beispielcode sehen.

Durch einige Ergänzungen der Upload-Funktion, die diese Funktion aufruft, werden die vom Benutzer hochgeladenen Word-Dokumente nun automatisch in das PDF-Format konvertiert.

Der folgende Code erstellt den Zielpfad für die konvertierte PDF und initiiert die Konvertierung:

```
const documentFolder = path.join(__dirname, "../docs");
var extPosition = req.files.source.name.lastIndexOf('.') - 1;
if(extPosition < 0 ) {
  extPosition = req.files.source.name.length
}
const destinationName = path.join(documentFolder,  
  req.files.source.name.substring(0, extPosition) + '.pdf');
console.log(destinationName);
 
logger.info('converting to ${destinationName}')
  convertDocumentToPDF(uploadPath, destinationName);
```

## Konvertieren anderer Dateitypen in PDF

Das Dokument-Toolkit konvertiert andere Dateiformate in PDF, z. B. das statische HTML, ein anderer gängiger Dokumenttyp. Das Toolkit akzeptiert das HTML von Dokumenten, die als ZIP-Datei verpackt sind, mit allen Ressourcen, auf die im Dokument (CSS-Dateien, Bilder und andere Dateien) in derselben ZIP-Datei verwiesen wird. Das HTML-Dokument selbst muss den Namen &quot;index.html&quot; haben und im Stammverzeichnis der ZIP-Datei abgelegt werden.

Verwenden Sie zum Konvertieren einer ZIP-Datei mit HTML den folgenden Code:

```
//Create an HTML to PDF operation and provide the source file to it
htmlToPDFOperation = PDFToolsSdk.CreatePDF.Operation.createNew();     
const input = PDFToolsSdk.FileRef.createFromLocalFile(sourceZipFile);
htmlToPDFOperation.setInput(input);
 
// custom function for setting options
setCustomOptions(htmlToPDFOperation);
 
// Execute the operation and Save the result to the specified location.
htmlToPDFOperation.execute(executionContext)
  .then(result => result.saveAsFile(destinationPdfFile))
  .catch(err => {
    logger.error('Exception encountered while executing operation');
});
```

Die Funktion &quot;`setCustomOptions`&quot; gibt andere PDF-Einstellungen an, z. B. die Seitengröße. Hier können Sie sehen, dass die Funktion die Seitengröße auf 11,5 x 11 Zoll setzt:

```
const setCustomOptions = (htmlToPDFOperation) => {    
  const pageLayout = new PDFToolsSdk.CreatePDF.options.PageLayout();
  pageLayout.setPageSize(11.5, 8);

  const htmlToPdfOptions = 
    new PDFToolsSdk.CreatePDF.options.html.CreatePDFFromHtmlOptions.Builder()
    .includesHeaderFooter(true)
    .withPageLayout(pageLayout)
    .build();
  htmlToPDFOperation.setOptions(htmlToPdfOptions);
};
```

Wenn Sie ein HTML-Dokument öffnen, das einige Begriffe enthält, erhalten Sie im Browser Folgendes:

![Bild der Computerbedingungen](assets/AWNjs_3.png)

Die Quelle für dieses Dokument besteht aus einer CSS-Datei und einer HTML-Datei:

![Image von CSS- und HTML-Datei](assets/AWNjs_4.png)

Nach dem Verarbeiten der HTML-Datei wird derselbe Text im PDF-Format angezeigt:

![PDF-Datei der Computerbedingungen](assets/AWNjs_5.png)

## Angehängte Seiten

Eine weitere häufige Funktion beim PDF von Dateien ist das Anhängen von Seiten an das Ende, die möglicherweise Standardtext enthalten, z. B. eine Terminologieliste. Das Dokument-Toolkit kann mehrere PDF-Dokumente zu einem einzigen Dokument zusammenführen. Wenn Sie über eine Liste von Dokumentpfaden verfügen (hier in `sourceFileList`), können Sie die Dateiverweise jeder Datei einem Objekt für einen Kombinationsvorgang hinzufügen.

Wenn der Kombinationsvorgang ausgeführt wird, wird eine einzelne Datei mit dem Inhalt der Quelle bereitgestellt. Sie können `saveAsFile` für das Objekt verwenden, um die Datei im Speicher zu speichern.

```
const executionContext = PDFToolsSDK.ExecutionContext.create(credentials);
var combineOperation = PDFToolsSDK.CombineFiles.Operation.createNew();
 
sourceFileList.forEach(f => {
  var combinedSource = PDFToolsSDK.FileRef.createFromLocalFile(f);
  console.log(f);
  combineOperation.addInput(combinedSource);
});
    
 
combineOperation.execute(executionContext)
  .then(result=>result.saveAsFile(destinationFile))
  .catch(err => {
    logger.error(err.message);
});    
```

## Anzeigen von PDF-Dokumenten

Sie haben mehrere Vorgänge zum PDF von Dateien ausgeführt, aber letztendlich muss der Benutzer die Dokumente anzeigen. Sie können das Dokument mithilfe der Adobe PDF Embed-API in eine Webseite einbetten.

Fügen Sie auf der Seite, auf der die PDF angezeigt wird, ein `<div />`-Element hinzu, das das Dokument enthält, und weisen Sie ihm eine ID zu. Du verwendest diese ID in Kürze. Fügen Sie auf der Webseite einen `<script />`-Verweis auf die Adobe-JavaScript-Bibliothek ein:

```
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

Der letzte Code, den Sie benötigen, ist eine Funktion, die das Dokument anzeigt, sobald der JavaScript-Code der Adobe PDF Embed-API geladen wurde. Wenn Sie eine Benachrichtigung erhalten, dass das Skript über ein adobe_dc_view\_sdk.ready-Ereignis geladen wird, erstellen Sie ein neues AdobeDC.View-Objekt. Dieses Objekt benötigt Ihre Client-ID und die ID des zuvor erstellten Elements. Suchen Sie Ihre Client-ID in [Adobe Developer Console](https://console.adobe.io/). Wenn Sie die Einstellungen für die Anwendung anzeigen, die Sie zuvor beim Generieren von Anmeldeinformationen erstellt haben, wird dort die Client-ID angezeigt.

![Abbild des API-Clientschlüssels](assets/AWNjs_6.png)

## Andere PDF-Optionen

Mit der [Adobe PDF Embed-API-Demo](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf) können Sie eine Vorschau der verschiedenen anderen Optionen zum Einbetten von PDF-Dokumenten anzeigen.

![Bild vom Einbetten der PDF-Optionen ](assets/AWNjs_7.png)

Sie können verschiedene Optionen aktivieren und deaktivieren und sofort sehen, wie sie gerendert werden. Wenn Ihnen eine Kombination gefällt, klicken Sie auf die Schaltfläche *\&lt;/\> Code generieren*, um den eigentlichen HTML-Code mithilfe dieser Optionen zu generieren.

![Bild der Codevorschau](assets/AWNjs_8.png)

## Digitale Signaturen und Sicherheit hinzufügen.

Sobald ein Dokument fertig ist, können Sie mit Adobe Sign digitale Signaturen zur Genehmigung hinzufügen. Diese Funktion funktioniert etwas anders als die bisher verwendete Funktion. Für digitale Signaturen muss eine Anwendung so konfiguriert sein, dass sie OAuth für die Benutzerauthentifizierung verwendet.

Der erste Schritt beim Einrichten Ihrer Anwendung besteht darin, [Ihre Anwendung zu registrieren](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/gstarted/create_app.md), um OAuth für Adobe Sign zu verwenden. Navigieren Sie nach der Anmeldung zum Bildschirm zum Erstellen von Anwendungen, indem Sie auf *Konto* klicken, dann den Abschnitt *Adobe Sign API* öffnen und auf *API-Anwendungen* klicken, um die Liste der registrierten Anwendungen zu öffnen.

![Bild des ersten Schritts bei der Registrierung Ihrer Anwendung](assets/AWNjs_9.png)

Um einen neuen Anwendungseintrag zu erstellen, klicken Sie auf das Pluszeichen in der oberen rechten Ecke.

![Bild des Pluszeichens in der oberen rechten Ecke des Bildschirms](assets/AWNjs_10.png)

Geben Sie im daraufhin geöffneten Fenster einen Anwendungsnamen und einen Anzeigenamen ein. Wählen Sie *Kunde* für die Domäne aus, und klicken Sie dann auf *Speichern*.

![Image des Ortes, an dem Anwendungsname und Anzeigename eingegeben werden sollen](assets/AWNjs_11.png)

Nachdem die Anwendung erstellt wurde, können Sie sie aus der Liste auswählen und auf *OAuth für Anwendung konfigurieren* klicken. Wählen Sie die Optionen aus. Geben Sie in das Feld &quot;Umleitungs-URL&quot; die URL für Ihre Anwendung ein. Hier können Sie mehrere URLs eingeben. Für die Anwendung, die Sie testen, lautet der Wert:

```
http://localhost:3000/signed-in 
```

Das Verfahren zur Verwendung von OAuth zum Abrufen eines Tokens ist Standard. Ihre Anwendung leitet einen Benutzer zu einer URL für die Anmeldung weiter. Nachdem sich der Benutzer erfolgreich angemeldet hat,
Sie werden mit zusätzlichen Informationen in den Abfrageparametern der Seite an die Anwendung zurückgeleitet.

Für die Anmelde-URL muss Ihre Anwendung Ihre Client-ID, die Umleitungs-URL und eine Liste der erforderlichen Geltungsbereiche übergeben.

Das Muster für die URL sieht folgendermaßen aus:

```
https://secure.adobesign.com/public/oauth?
  redirect_uri=&
  response_type=code&
  client_id=&
  scope=
```

Der Benutzer wird aufgefordert, sich bei seiner ID für Adobe Sign anzumelden. Nach der Anmeldung werden sie gefragt, ob sie der Anwendung Berechtigungen erteilen möchten.

![Bild des Bildschirms zur Bestätigung des Zugriffs](assets/AWNjs_12.png)

Wenn der Benutzer auf der Umleitungs-URL auf *Zugriff zulassen* klickt, übergibt ein Abfrageparameter mit dem Namen Code den Autorisierungscode:

https://YourServer.com/?code=**\&lt;Autorisierungscode\>**\&amp;api_access_point=https://api.adobesign.com&amp;web_access_point=https://secure.adobesign.com

Wenn Sie diesen Code zusammen mit Ihrer Client-ID und dem Client-Schlüssel an den Adobe Sign-Server senden, wird ein Zugriffstoken für den Zugriff auf den Dienst bereitgestellt. Speichern Sie die Werte in den Parametern `api_access_point` und `web_access_point`. Diese Werte werden für weitere Anforderungen verwendet.

```
var requestURL = ' ${api_access_point}oauth/token?code=${code}'
  +'&client_id=${client_id}'
  +'&client_secret=${client_secret}&'
  +'redirect_uri=${redirect_url}&'
  +'grant_type=authorization_code';
request.post(requestURL, {form: { }
}, (err,response,body)=>{                
    var token_response = JSON.parse(body)
    var access_token = token_response.access_token;
    console.log(access_token);
});
```

Wenn ein Dokument signiert werden muss, muss es zuerst hochgeladen werden. Ihre Anwendung kann das Dokument auf den `api_access_point`-Wert hochladen, der beim Anfordern des OAUTH-Tokens empfangen wurde. Der Endpunkt ist `/api/rest/v6/transientDocuments`. Die Anforderungsdaten sehen wie folgt aus:

```
POST /api/rest/v6/transientDocuments HTTP/1.1
Host: api.na1.adobesign.com
Authorization: Bearer MvyABjNotARealTokenHkYyi
Content-Type: multipart/form-data
Content-Disposition: form-data; name=";File"; filename="MyPDF.pdf"
<PDF CONTENT>
```

Erstellen Sie die Anforderung in der Anwendung mit dem folgenden Code:

```
var uploadRequest = {
  'method': 'POST',
  'url': '${oauthParameters.signin_domain}/api/rest/v6/transientDocuments',
  'headers': {
    'Authorization': 'Bearer  ${auth_token}'
  },
  formData: {
    'File': {
      'value': fs.createReadStream(documentPath),
      'options': {
        'filename': fileName,
        'contentType': null
      }
    }
  }
};
 
request(uploadRequest, (error, response) => {
  if (error) throw new Error(error);
  var jsonResponse = JSON.parse(response.body);
  var transientDocumentId = jsonResponse.transientDocumentId;
  logger.info('transientDocumentId:', transientDocumentId)
});
```

Die Anforderung gibt einen `transientID`-Wert zurück. Das Dokument wurde hochgeladen, wurde aber noch nicht gesendet. Verwenden Sie zum Senden des Dokuments `transientID`, um das Senden des Dokuments anzufordern.

Erstellen Sie zunächst ein JSON-Objekt, das die Informationen für das zu signierende Dokument enthält. Im Folgenden enthält die Variable &quot;`transientDocumentId`&quot; die ID aus dem obigen Code und &quot;`agreementDescription`&quot; den Text, der die zu signierende Vereinbarung beschreibt. Die Personen, die das Dokument signieren sollen, werden in `participantSetsInfo` nach ihrer E-Mail-Adresse und Rolle aufgelistet.

```
var requestBody = {
  "fileInfos":[
    {"transientDocumentId":transientDocumentId}],
    "name":agreementDescription,
    "participantSetsInfo":[
      {"memberInfos":[{"email":"user@domain.com"}],
       "order":1,"role":"SIGNER"}
    ],
    "signatureType":"ESIGN","state":"IN_PROCESS"
};
```

Durch Senden dieser Webanforderung wird die Signieranforderung erstellt und ein JSON-Objekt mit einer ID für die Vereinbarungsanforderung zurückgegeben:

```
request(requestBody, function (error, response) {
  if (error) throw new Error(error);
  var JSONResponse = JSON.parse(response.body);
  var requestId = JSONResponse.id;
});
```

Wenn die Unterzeichner die Signatur vergessen haben und eine weitere Benachrichtigungs-E-Mail benötigen, senden Sie die Benachrichtigungen erneut mit der zuvor erhaltenen ID. Der einzige Unterschied besteht darin, dass Sie auch die Teilnehmer-IDs der Parteien hinzufügen müssen. Sie können die Teilnehmer-IDs abrufen, indem Sie eine GET-Anforderung an `/agreements/{agreementID}/members` senden.

Um das Senden der Erinnerung anzufordern, erstellen Sie zunächst ein JSON-Objekt, das die Anforderung beschreibt. Das Minimalobjekt benötigt eine Liste der Teilnehmer-IDs und einen Status für die Erinnerung (&quot;AKTIV&quot;, &quot;ABGESCHLOSSEN&quot; oder &quot;ABGEBROCHEN&quot;).

Die Anforderung kann optional zusätzliche Informationen enthalten, z. B. einen Wert für &quot;note&quot;, der dem Benutzer angezeigt wird. Oder eine Verzögerung (in Stunden), bis die Erinnerung gesendet wird (in `firstReminderDelay`), und eine Erinnerungshäufigkeit (im Feld &quot;Häufigkeit&quot;), die Werte wie DAILY_UNTIL_SIGNED, EVERY_THIRD_DAY_UNTIL_SIGNED oder WEEKLY_UNTIL_SIGNED akzeptiert.

```
var requestBody = {
  //participantList is an array of participant ID strings
  "recipientParticipantIds":participantList
  ,"status":"ACTIVE",
  "note":"This is a reminder to sign out important agreement."
}
 
var reminderRequest = {
  'method': 'POST',
  'url': '${oauthParameters.signin_domain}/api/rest/v6/agreements/${agreementID}/reminders',
  'headers': {
    'Authorization': `Bearer ${access_token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(requestBody)
 
};

request(reminderRequest, function (error, response) {
});
```

Und das ist alles, was nötig ist, um eine Erinnerungsanfrage zu senden.

![Bild des Webformulars](assets/AWNjs_13.png)

## Erstellen von Webformularen

Sie können die Adobe Sign-API auch zum Erstellen von Webformularen verwenden. Mit Webformularen können Sie ein Formular in eine Webseite einbetten oder direkt mit dieser verknüpfen. Nachdem ein Webformular erstellt wurde, wird es auch in den Webformularen in der Adobe Sign-Konsole angezeigt. Sie können Webformulare mit dem Status ENTWURF für die inkrementelle Erstellung, dem Status AUTHORING für die Bearbeitung der Webformularfelder und dem Status AKTIV erstellen, um das Formular sofort zu hosten.

![Bild eines Webformulars im Adobe Sign-Verwaltungsbildschirm](assets/AWNjs_14.png)

Verwenden Sie zum Erstellen eines Webformulars das Formular `transientDocumentId`. Legen Sie einen Titel für das Formular und einen Status für die Initialisierung fest.

```
var requestBody = {
  "fileInfos": [
    {
      "transientDocumentId": transientDocumentId
    }
  ],
  "name": webFormTitle,
  "state": status,
  "widgetParticipantSetInfo": {
    "memberInfos": [ { "email": "" } ],
    "role": "SIGNER"
  }
}
```

```
var createWebFormRequest = {
  'method': 'POST',
  'url': `${oauthParameters.signin_domain}/api/rest/v6/widgets`,
  'headers': {
    'Authorization': `Bearer ${access_token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(requestBody)
}
```

```
request(createWebFormRequest, function (error, response) {
  var jsonResp = JSON.parse(response.body);
  var webFormID = jsonResp.id;
});
```

Sie können jetzt ein Dokument einbetten oder eine Verknüpfung zu diesem erstellen.

## Nächste Schritte

Wie Sie an den Schnellstarts und dem bereitgestellten Code sehen können, ist es leicht, PDF- und digitale Dokumentengenehmigungsprozesse mithilfe von Node mit den [!DNL Adobe Acrobat Services]-APIs zu implementieren. Die APIs von Adobe lassen sich nahtlos in Ihre bestehenden Client-Anwendungen integrieren.

Um die erforderlichen Bereiche für einen Aufruf zu ermitteln oder zu sehen, wie der Aufruf erstellt wird, können Sie Beispielaufrufe aus der [REST-API-Dokumentation](https://secure.na4.adobesign.com/public/docs/restapi/v6) erstellen. Die [Quickstarts](https://github.com/adobe/pdftools-node-sdk-samples) veranschaulichen auch andere Funktionen und Dateiformate, die von den [!DNL Adobe Acrobat Services]-APIs verarbeitet werden.

Sie können Ihren Anwendungen eine Vielzahl von PDF-Funktionen hinzufügen, mit denen Ihre Benutzer ihre Dokumente schnell und einfach anzeigen und signieren können und vieles mehr. Beginnen Sie noch heute mit [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/).
