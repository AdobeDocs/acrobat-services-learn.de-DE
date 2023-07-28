---
title: Vereinbarungsarbeitsabläufe in Node.js
description: "[!DNL Adobe Acrobat Services] APIs integrieren PDF-Funktionen ganz einfach in Ihre Webanwendungen."
type: Tutorial
role: Developer
level: Beginner
feature: Use Cases
thumbnail: KT-7473.jpg
jira: KT-7473
keywords: Empfohlen
exl-id: 44a03420-e963-472b-aeb8-290422c8d767
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '2182'
ht-degree: 1%

---

# Vereinbarungs-Workflows in Node.js

![Use Case Hero-Banner](assets/UseCaseAgreementHero.jpg)

Viele Geschäftsanwendungen und -prozesse erfordern Dokumentationen wie Angebote und Vereinbarungen. Das PDF von Dokumenten stellt sicher, dass Dateien sicherer und weniger veränderbar sind. Zusätzlich werden digitale Signaturen unterstützt, sodass Kunden ihre Dokumente schnell und einfach ausfüllen können. [!DNL Adobe Acrobat Services] APIs integrieren PDF-Funktionen ganz einfach in Ihre Webanwendungen.

## Lernziel.

In diesem Tutorial lernen Sie, wie Sie PDF-Services zu einer Anwendung &quot;Node.js&quot; hinzufügen, um einen Vereinbarungsprozess zu digitalisieren.

## Relevante APIs und Ressourcen

* [PDF Services-API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF Embed-API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Adobe Sign-API](https://www.adobe.io/apis/documentcloud/sign.html)

* [Projektcode](https://github.com/adobe/pdftools-node-sdk-samples)

## Einrichten [!DNL Adobe Acrobat Services]

Richten Sie zunächst die zu verwendenden Anmeldeinformationen ein [!DNL Adobe Acrobat Services]. Registrieren Sie ein Konto und verwenden Sie die [Node.js QuickStart](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#node-js) , um zu überprüfen, ob Ihre Anmeldeinformationen funktionieren, bevor Sie die Funktionen in eine größere Anwendung integrieren.

Besorgen Sie sich zunächst ein Adobe-Entwicklerkonto. Klicken Sie dann auf der Registerkarte [Erste Schritte](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK) &quot; die Option *Erste Schritte* unter &quot;Neue Anmeldeinformationen erstellen&quot;. Sie können sich für die kostenlose Testversion anmelden, die 1.000 Dokumenttransaktionen bietet, die über sechs Monate verwendet werden können.

![Image der Erstellung neuer Anmeldeinformationen](assets/AWNjs_1.png)

Auf der folgenden Seite Neue Anmeldeinformationen erstellen werden Sie aufgefordert, zwischen der PDF Embed-API und der PDF Services-API zu entscheiden.

Auswählen *PDF Services-API*.

Geben Sie einen Namen für die Anwendung ein und aktivieren Sie das Kontrollkästchen *Erstellen eines personalisierten Codebeispiels*. Wenn Sie dieses Kontrollkästchen aktivieren, werden Ihre Anmeldeinformationen automatisch in das Codebeispiel eingebettet. Wenn Sie dieses Kontrollkästchen deaktiviert lassen, müssen Sie der Anwendung manuell Ihre Anmeldeinformationen hinzufügen.

Auswählen *Node.js* für den Anwendungstyp und klicken Sie auf *Anmeldeinformationen erstellen*.

Einige Augenblicke später wird eine ZIP-Datei mit einem Beispielprojekt einschließlich Ihrer Anmeldedaten heruntergeladen. Das Paket Node.js für [!DNL Acrobat Services] ist bereits als Teil des Beispielprojektcodes enthalten.

![Bild der Auswahl der API-Zugangsberechtigungen für PDF-Dienste](assets/AWNjs_2.png)

## Manuelles Konfigurieren des Beispielprojekts

Wenn Sie kein Beispielprojekt von der Seite &quot;Neue Anmeldeinformationen erstellen&quot; herunterladen, können Sie das Projekt auch manuell einrichten.

Laden Sie den Code (ohne Ihre Anmeldeinformationen einzubetten) von der [GitHub](https://github.com/adobe/pdftools-node-sdk-samples). Wenn Sie diese Version des Codes verwenden, müssen Sie Ihre Anmeldeinformationen vor der Verwendung der Datei pdftools-api-credentials.json hinzufügen:

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

Sie müssen das Paket Node.js für [!DNL Acrobat Services]. Verwenden Sie den folgenden Befehl, um das Paket zu installieren:

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

Konvertieren Sie als Nächstes die Datei vom nativen Format in das PDF-Format. Der zuvor heruntergeladene Beispielcode enthält ein Skript mit dem Namen `create-pdf-from-docx.js` zum Konvertieren eines Dokuments in das PDF-Format. Die folgende Funktion `convertDocumentToPDF`, verwendet ein hochgeladenes Dokument und konvertiert es in eine PDF in einem anderen Ordner:

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

Die Funktion `setCustomOptions` Legt andere PDF-Einstellungen fest, z. B. das Seitenformat. Hier können Sie sehen, dass die Funktion die Seitengröße auf 11,5 x 11 Zoll setzt:

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

![Bild von Computerbegriffen](assets/AWNjs_3.png)

Die Quelle für dieses Dokument besteht aus einer CSS-Datei und einer HTML-Datei:

![Bild einer CSS- und HTML-Datei](assets/AWNjs_4.png)

Nach dem Verarbeiten der HTML-Datei wird derselbe Text im PDF-Format angezeigt:

![PDF-Datei mit Computerbegriffen](assets/AWNjs_5.png)

## Angehängte Seiten

Eine weitere häufige Funktion beim PDF von Dateien ist das Anhängen von Seiten an das Ende, die möglicherweise Standardtext enthalten, z. B. eine Terminologieliste. Das Dokument-Toolkit kann mehrere PDF-Dokumente zu einem einzigen Dokument zusammenführen. Wenn Sie eine Liste von Dokumentpfaden haben (hier in `sourceFileList`), können Sie die Dateiverweise jeder Datei zu einem Objekt für einen Kombinationsvorgang hinzufügen.

Wenn der Kombinationsvorgang ausgeführt wird, wird eine einzelne Datei mit dem Inhalt der Quelle bereitgestellt. Sie können `saveAsFile` auf dem Objekt, um die Datei im Speicher zu speichern.

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

Sie haben mehrere Vorgänge zum PDF von Dateien ausgeführt, aber letztendlich muss der Benutzer die Dokumente anzeigen. Sie können das Dokument mithilfe der PDF Embed-API von Adobe in eine Webseite einbetten.

Fügen Sie auf der Seite, auf der die PDF angezeigt wird, eine `<div />` -Element, um das Dokument zu speichern und ihm eine ID zu geben. Du verwendest diese ID in Kürze. Fügen Sie auf der Webseite ein `<script />` Verweis auf die JavaScript-Bibliothek der Adobe:

```
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

Der letzte Code, den Sie benötigen, ist eine Funktion, die das Dokument anzeigt, sobald der JavaScript-Code der Adobe PDF Embed-API geladen wurde. Wenn Sie eine Benachrichtigung erhalten, dass das Skript über ein adobe_dc_view\_sdk.ready-Ereignis geladen wird, erstellen Sie ein neues AdobeDC.View-Objekt. Dieses Objekt benötigt Ihre Client-ID und die ID des zuvor erstellten Elements. Suchen Sie Ihre Client-ID im [Adobe Developer Console](https://console.adobe.io/). Wenn Sie die Einstellungen für die Anwendung anzeigen, die Sie zuvor beim Generieren von Anmeldeinformationen erstellt haben, wird dort die Client-ID angezeigt.

![Bild des API-Clientschlüssels](assets/AWNjs_6.png)

## Andere PDF-Optionen

Die [Demo zur Adobe PDF Embed-API](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf) können Sie eine Vorschau der verschiedenen anderen Optionen zum Einbetten von PDF-Dokumenten anzeigen.

![Bild vom Einbetten von PDF-Optionen ](assets/AWNjs_7.png)

Sie können verschiedene Optionen aktivieren und deaktivieren und sofort sehen, wie sie gerendert werden. Wenn Ihnen eine Kombination gefällt, klicken Sie auf das Symbol *\&lt;/\> Code generieren* , um den eigentlichen HTML-Code mithilfe dieser Optionen zu generieren.

![Bild der Codevorschau](assets/AWNjs_8.png)

## Digitale Signaturen und Sicherheit hinzufügen.

Sobald ein Dokument fertig ist, können Sie mit Adobe Sign digitale Signaturen zur Genehmigung hinzufügen. Diese Funktion funktioniert etwas anders als die bisher verwendete Funktion. Für digitale Signaturen muss eine Anwendung so konfiguriert sein, dass sie OAuth für die Benutzerauthentifizierung verwendet.

Der erste Schritt beim Einrichten Ihrer Anwendung besteht darin, [Bewerbung registrieren](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/gstarted/create_app.md) , um OAuth für Adobe Sign zu verwenden. Navigieren Sie nach der Anmeldung zum Bildschirm für die Anwendungserstellung, indem Sie auf *Konto*, öffnen Sie dann das Dialogfeld *ADOBE SIGN API* und klicken Sie auf *API-Anwendungen* , um die Liste der registrierten Anwendungen zu öffnen.

![Bild des ersten Schritts bei der Registrierung Ihrer Anwendung](assets/AWNjs_9.png)

Um einen neuen Anwendungseintrag zu erstellen, klicken Sie auf das Pluszeichen in der oberen rechten Ecke.

![Abbildung des Pluszeichens in der rechten oberen Ecke des Bildschirms](assets/AWNjs_10.png)

Geben Sie im daraufhin geöffneten Fenster einen Anwendungsnamen und einen Anzeigenamen ein. Auswählen *Kunde* für die Domäne ein, und klicken Sie dann auf *Speichern*.

![Bild des Ortes, an dem der Anwendungsname und der Anzeigename eingegeben werden müssen](assets/AWNjs_11.png)

Nachdem die Anwendung erstellt wurde, können Sie sie aus der Liste auswählen und auf *OAuth für Anwendung konfigurieren*. Wählen Sie die Optionen aus. Geben Sie in das Feld &quot;Umleitungs-URL&quot; die URL für Ihre Anwendung ein. Hier können Sie mehrere URLs eingeben. Für die Anwendung, die Sie testen, lautet der Wert:

```
http://localhost:3000/signed-in 
```

Das Verfahren zur Verwendung von OAuth zum Abrufen eines Tokens ist Standard. Ihre Anwendung leitet einen Benutzer zu einer URL für die Anmeldung weiter. Nachdem sich der Benutzer erfolgreich angemeldet hat, wird er mit zusätzlichen Informationen in den Abfrageparametern der Seite zurück zur Anwendung geleitet.

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

![Bild des Bestätigungsbildschirms](assets/AWNjs_12.png)

Wenn der Benutzer auf *Zugriff erlauben* an der Umleitungs-URL übergibt ein Abfrageparameter namens Code den Autorisierungscode:

https://YourServer.com/?code=**\&lt;authorization_code>**\&amp;api_access_point=https://api.adobesign.com&amp;web_access_point=https://secure.adobesign.com

Wenn Sie diesen Code zusammen mit Ihrer Client-ID und dem Client-Schlüssel an den Adobe Sign-Server senden, wird ein Zugriffstoken für den Zugriff auf den Dienst bereitgestellt. Speichern der Werte in den Parametern `api_access_point` und `web_access_point`. Diese Werte werden für weitere Anforderungen verwendet.

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

Wenn ein Dokument signiert werden muss, muss es zuerst hochgeladen werden. Ihre Anwendung kann das Dokument in die Datei `api_access_point` Wert, der beim Anfordern des OAUTH-Tokens empfangen wurde. Der Endpunkt ist `/api/rest/v6/transientDocuments`. Die Anforderungsdaten sehen wie folgt aus:

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

Die Anforderung gibt eine `transientID` Wert. Das Dokument wurde hochgeladen, wurde aber noch nicht gesendet. Um das Dokument zu senden, verwenden Sie den Befehl `transientID` , um den Versand des Dokuments anzufordern.

Erstellen Sie zunächst ein JSON-Objekt, das die Informationen für das zu signierende Dokument enthält. Die Variable `transientDocumentId` enthält die ID aus dem obigen Code und `agreementDescription` enthält Text zur Beschreibung der Vereinbarung, die signiert werden muss. Die Personen, die das Dokument signieren sollen, sind in `participantSetsInfo` nach ihrer E-Mail-Adresse und Rolle.

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

Wenn die Unterzeichner die Signatur vergessen haben und eine weitere Benachrichtigungs-E-Mail benötigen, senden Sie die Benachrichtigungen erneut mit der zuvor erhaltenen ID. Der einzige Unterschied besteht darin, dass Sie auch die Teilnehmer-IDs der Parteien hinzufügen müssen. Sie können die Teilnehmer-IDs abrufen, indem Sie eine GET-Anforderung an `/agreements/{agreementID}/members`.

Um das Senden der Erinnerung anzufordern, erstellen Sie zunächst ein JSON-Objekt, das die Anforderung beschreibt. Das Minimalobjekt benötigt eine Liste der Teilnehmer-IDs und einen Status für die Erinnerung (&quot;AKTIV&quot;, &quot;ABGESCHLOSSEN&quot; oder &quot;ABGEBROCHEN&quot;).

Die Anforderung kann optional zusätzliche Informationen enthalten, z. B. einen Wert für &quot;note&quot;, der dem Benutzer angezeigt wird. Oder eine Verzögerung (in Stunden), um zu warten, bis die Erinnerung gesendet wird (in `firstReminderDelay`) und eine Erinnerungshäufigkeit (im Feld &quot;Häufigkeit&quot;), die Werte wie DAILY_UNTIL_SIGNED, EVERY_THIRD_DAY_UNTIL_SIGNED oder WEEKLY_UNTIL_SIGNED akzeptiert.

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

![Bild eines Webformulars](assets/AWNjs_13.png)

## Erstellen von Webformularen

Sie können die Adobe Sign-API auch zum Erstellen von Webformularen verwenden. Mit Webformularen können Sie ein Formular in eine Webseite einbetten oder direkt mit dieser verknüpfen. Nachdem ein Webformular erstellt wurde, wird es auch in den Webformularen in der Adobe Sign-Konsole angezeigt. Sie können Webformulare mit dem Status ENTWURF für die inkrementelle Erstellung, dem Status AUTHORING für die Bearbeitung der Webformularfelder und dem Status AKTIV erstellen, um das Formular sofort zu hosten.

![Bild eines Webformulars auf der Adobe Sign-Seite &quot;Verwalten&quot;](assets/AWNjs_14.png)

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

Wie Sie an den Quick Starts und dem bereitgestellten Code sehen können, ist es leicht, PDF- und digitale Dokumentengenehmigungsprozesse über Node mit dem [!DNL Adobe Acrobat Services] APIs Die APIs von Adobe lassen sich nahtlos in Ihre bestehenden Client-Anwendungen integrieren.

Um die erforderlichen Geltungsbereiche für einen Aufruf zu ermitteln oder zu sehen, wie der Aufruf erstellt wird, können Sie Beispielaufrufe aus der Datei [REST-API-Dokumentation](https://secure.na4.adobesign.com/public/docs/restapi/v6). Die [Quickstarts](https://github.com/adobe/pdftools-node-sdk-samples) demonstrieren Sie auch andere Funktionen und Dateiformate im [!DNL Adobe Acrobat Services] APIs-Prozesse

Sie können Ihren Anwendungen eine Vielzahl von PDF-Funktionen hinzufügen, mit denen Ihre Benutzer ihre Dokumente schnell und einfach anzeigen und signieren können und vieles mehr. Checken Sie zum Starten [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) noch heute.
