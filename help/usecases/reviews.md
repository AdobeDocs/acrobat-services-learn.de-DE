---
title: Überprüfungen und Genehmigungen
description: Workflow zur Abstimmung und Genehmigung von Dokumenten im Team lernen.
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8094
thumbnail: KT-8094.jpg
exl-id: d704620f-d06a-4714-9d09-3624ac0fcd3a
source-git-commit: b7a20f30a2eb175053c7a25be0411f80dd88899f
workflow-type: tm+mt
source-wordcount: '1540'
ht-degree: 0%

---

# Überprüfungen und Genehmigungen

![Banner &quot;Use Case Hero&quot;](assets/UseCaseReviewsHero.jpg)

Während der COVID-19-Pandemie war für viele Unternehmen die Zusammenarbeit im Team über mehrere Standorte hinweg erforderlich. [Das Freigeben und Überprüfen digitaler Dokumente](https://developer.adobe.com/document-services/use-cases/collaboration/review-and-approval) stellt eine Reihe von Herausforderungen für Teams und funktionsübergreifende Ressourcen dar.

Diese Herausforderungen umfassen das Freigeben von Dokumenten in verschiedenen Dateiformaten, das effektive Überprüfen und Kommentieren des Inhalts sowie die Synchronisierung mit den neuesten Bearbeitungen. [!DNL Adobe Acrobat Services] APIs sind so konzipiert, dass Anwendungsentwickler diese Herausforderungen für ihre Benutzer lösen können.

## Lernziel.

Dieses praktische Tutorial zeigt, wie Sie einen Workflow für die Überprüfung und Genehmigung von Dokumenten in einer Node.js- und einer Express-Webanwendung erstellen. Um dieses Tutorial nachzuvollziehen, benötigen Sie Erfahrung mit Node.js.

Die Anwendung verfügt über die folgenden Funktionen:

* Konvertieren verschiedener Dateitypen in PDF

* Datei-Uploads aktivieren

* Benutzern die Möglichkeit geben, Kommentare und Anmerkungen hinzuzufügen

* Die PDF sowie die Kommentare anzeigen

* Benutzerprofile zur Identifizierung von Kommentarautoren aktivieren

* Dateien zu einer finalen PDF zusammenführen, die Anwender herunterladen können

## Relevante APIs und Ressourcen

* [PDF Services-API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF Embed-API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Projektcode](https://github.com/contentlab-io/adobe_reviews_and_approvals)

## Erstellen von Adobe API-Zugangsberechtigungen

Bevor Sie den Code starten, müssen Sie [Anmeldeinformationen](https://www.adobe.com/go/dcsdks_credentials) für die Adobe PDF Embed-API und die Adobe PDF Services-API erstellen. Die PDF Embed-API kann kostenlos verwendet werden. Die PDF Services-API kann sechs Monate lang kostenlos genutzt werden. Dann können Sie zu einem [Pay-as-you-go-Plan](https://developer.adobe.com/document-services/pricing/main) mit nur \$0,05 pro Dokumenttransaktion wechseln.

Wählen Sie beim Erstellen von Anmeldeinformationen für die PDF Services-API die Option **Personalisiertes Codebeispiel erstellen** und wählen Sie als Sprache &quot;Node.js&quot; aus. Speichern Sie die ZIP-Datei und extrahieren Sie pdftools-api-credentials.json und private.key in das Stammverzeichnis Ihres Node.js Express-Projekts.

## Projekt und Abhängigkeiten einrichten

Richten Sie das Projekt &quot;Node.js&quot; und das Express-Projekt so ein, dass statische Dateien aus einem Ordner mit dem Namen &quot;public&quot; bereitgestellt werden. Sie können Ihre Projektmethoden entsprechend Ihren Vorlieben einrichten. Um schnell loszulegen, können Sie den [Express-App-Generator &#x200B;](https://expressjs.com/en/starter/generator.html) verwenden. Wenn Sie die Dinge einfach halten möchten, können Sie [ganz von vorne beginnen](https://expressjs.com/en/starter/hello-world.html) und den Code in einer einzigen JavaScript-Datei speichern. Im oben verknüpften Beispielprojekt verwenden Sie den Ein-Datei-Ansatz und behalten den gesamten Code in der Datei &quot;index.js&quot;.

Kopieren Sie die Dateien `pdftools-api-credentials.json` und `private.key` aus dem personalisierten Codebeispiel in das Stammverzeichnis des Projekts. Fügen Sie sie auch der .gitignore-Datei hinzu, sofern vorhanden, sodass Ihre Anmeldedaten nicht versehentlich an ein Repository gesendet werden.

Führen Sie als Nächstes `npm install @adobe/documentservices-pdftools-node-sdk` aus, um das SDK &quot;Node.js&quot; für PDF Services zu installieren. Importieren Sie dieses Modul und erstellen Sie das API-Zugangsberechtigungsobjekt in Ihrem Code (index.js in Ihrem Beispielprojekt), nachdem der Rest Ihrer Abhängigkeit wie folgt importiert wurde:

```
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();
```

Der Startcode sollte wie folgt aussehen:

```
  
  const express = require( "express" );
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();

  const app = express();

  app.use( express.static( "public" ) );

  app.listen( 8889, function() {
      console.log( "Server started on port", 8889 );
  } );
```

Jetzt können Sie mit [!DNL Acrobat Services] APIs arbeiten.

## Konvertieren einer Datei auf einen PDF

Für den ersten Teil des Dokumenten-Workflows muss der Endbenutzer Dokumente hochladen, um sie freizugeben. Um diese zu aktivieren, fügen Sie eine Upload-Funktion hinzu und konsolidieren die verschiedenen Dokumentdateiformate in PDF, um sie für den Review-Prozess vorzubereiten.

Erstellen Sie zunächst eine Funktion zum Konvertieren von Dokumenten in PDF, die auf dem [-Beispielfragment für die PDF Services-API &#x200B;](https://developer.adobe.com/document-services/apis/pdf-services) basiert. In diesem Beispiel werden außerdem Snippets für viele andere wichtige Funktionen angezeigt, darunter die optische Zeichenerkennung (OCR), Kennwortschutz und -entfernung sowie Komprimierung.

```
function fileToPDF( filename, outputFilename, callback ) {
      // Create an ExecutionContext using credentials and create a new operation
  instance.
      const executionContext = PDFToolsSdk.ExecutionContext.create( credentials ),
          createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();

      // Set operation input from a source file.
      const input = PDFToolsSdk.FileRef.createFromLocalFile( filename );
      createPdfOperation.setInput( input );

      // Execute the operation and Save the result to the specified location.
      createPdfOperation.execute( executionContext )
          .then( result => {
              result.saveAsFile( outputFilename );
              callback( outputFilename );
          } );
  }
```

Sie können diese Funktion jetzt verwenden, um PDF aus hochgeladenen Dokumenten zu erstellen.

## Verarbeitung von Dateiuploads

Als Nächstes benötigt der Server einen Datei-Upload-Endpunkt auf dem Webserver, um die Dokumente zu empfangen und zu verarbeiten.

Erstelle zunächst einen Ordner innerhalb eines Uploads-Ordners, und nenne ihn &quot;drafts&quot;. Die hochgeladenen Dateien und die konvertierten PDF-Dateien werden hier gespeichert. Führen Sie als Nächstes `npm install express-fileupload` aus, um das Express-FileUpload-Modul zu installieren und die Middleware zu Express in Ihrem Code hinzuzufügen:

```
const fileUpload = require( "express-fileupload" );
app.use( fileUpload() );
```

Fügen Sie nun einen `/upload`-Endpunkt hinzu und speichern Sie die hochgeladene Datei unter demselben Dateinamen im Entwurfsordner. Rufen Sie dann die vorher geschriebene Funktion auf, um eine PDF-Datei desselben Dokuments zu erstellen, falls sie noch nicht im PDF-Format vorliegt. Sie können einen Dateinamen für die neue PDF-Datei erstellen, der auf dem Namen des hochgeladenen Originaldokuments basiert:

```
// Create a PDF file from an uploaded file
app.post( "/upload", ( req, res ) => {
    if( !req.files || Object.keys( req.files ).length === 0 ) {
        return res.status( 400 ).send( "No files were uploaded." );
    }
    
    // Create PDF from the uploaded file
    let file = req.files.myFile;
    file.mv( __dirname + "/uploads/drafts/" + file.name, ( err ) => {
        if( err ) {
            return res.status( 500 ).send( err );
        }
        if( file.name.endsWith( ".pdf" ) ) {
            res.redirect( "/" );
        }
        else {
            // Convert to PDF
            fileToPDF( __dirname + "/uploads/drafts/" + file.name, __dirname + "/uploads/drafts/" + file.name.replace( /\./g, "-" ) + ".pdf", ( file ) => {
                res.redirect( "/" );
            } );
        }
    });
} );
```

## Upload-Seite erstellen

Erstellen Sie jetzt zum Hochladen von Dateien aus der Webanwendung eine `index.html`-Webseite im Ordner &quot;uploads&quot;. Fügen Sie auf der Seite ein Formular für den Datei-Upload hinzu, das die Datei an den Endpunkt /upload sendet:

```
<form ref="uploadForm" 
      action="/upload"
      method="post" 
      encType="multipart/form-data">
      <input type="file" name="myFile" accept=".doc,.docx,.ppt,.pptx,.xls,.xlsx,.txt,.rtf,.bmp,.jpg,.gif,.tiff,.png">
      <input type="submit" value="Upload File" />
  </form>
```

![Screenshot der Funktion zum Hochladen von Dateien auf der Webseite &#x200B;](assets/reviews_1.png)

Sie können jetzt Dokumente auf den Server Node.js hochladen. Der Server speichert die Datei im Ordner &quot;uploads/drafts&quot; und erstellt daneben eine PDF-Format-Version.

Sie können die hochgeladenen Dokumente jetzt einbetten. Verwenden Sie daher die PDF Embed-API, um es Benutzern zu ermöglichen, den Dokumenten auf einfache Weise Kommentare und Anmerkungen hinzuzufügen.

## Auflisten von PDF-Dateien

Da ein typischer Dokumenten-Workflow mehrere Dokumente umfassen kann, müssen Sie eine Liste von Dokumenten anzeigen und jedes Dokument mit einer neuen Dokumentüberprüfungsseite in Ihrer App verknüpfen.

Fügen Sie zunächst im Servercode einen /files -Endpunkt hinzu, der eine Liste aller PDF-Dateien abruft und zurückgibt, die im Ordner &quot;uploads/drafts&quot; gespeichert sind:

```
const fs = require( "fs" );

app.get( "/files", ( req, res ) =\> {

fs.readdir( \_\_dirname + "/uploads/drafts/", ( err, files ) =\> {

if( err ) {

return res.status( 500 ).send( err );using

}

return res.json( files.filter( f =\> f.endsWith( ".pdf" ) ) );

} );

} );
```

Fügen Sie eine `/download/:file`-Route hinzu, die Zugriff auf die hochgeladene PDF-Datei zum Einbetten in die Webseite bietet.

>[!NOTE]
>
>In einer Produktionsanwendung müssen Sie Authentifizierung und Autorisierung hinzufügen, um sicherzustellen, dass die Anforderung von einem gültigen Benutzer stammt und dass der Benutzer auf das Dokument zugreifen darf.

```
app.get( "/download/:file", function( req, res ){
    // Note: In production code, this should check authentication and user access permissions
    res.download( __dirname + "/uploads/drafts/" + req.params[ "file" ] );
});
```

Aktualisieren Sie die Seite &quot;index.html&quot; mit einem Dateilistenelement, das zur Ladezeit gefüllt wird. Jedes Element kann mit einer draft.html-Webseite verknüpft werden, und Sie übergeben den Dateinamen mithilfe von Abfragezeichenfolgen-Parametern an die Seite.

>[!NOTE]
>
>Jedes Element wird mit jQuery angehängt. Du musst also die jQuery-Bibliothek auf deine Webseite laden oder das Element mit einer anderen Methode anhängen.

```
  <ul id="filelist">
      <li>Loading documents...</li>
  </ul>

  ...

  <script>
      // Load current files
      fetch( "/files" )
      .then( r => r.json() )
      .then( files => {
          if( files && files.length > 0 ) {
              $( "#filelist" ).empty();
              files.forEach( file => {
                  $( "#filelist" ).append( `<li><a
  href="/draft.html?file=${file}">${file}</a></li>` );
              })
          } else {
                  $("#filelist").append("<div>No documents found.</div>");
                }
      });
  </script>
```

![Screenshot der Auswahl einer Datei für die Überprüfung](assets/reviews_2.png)

## Einbetten einer PDF

Sie können jetzt PDF-Dateien in Ihre Webanwendung einbetten und anzeigen.

Erstelle eine Webseite mit dem Namen &quot;draft.html&quot;. Füge ein div-Element zur eingebetteten PDF hinzu:

```
  <div id="adobe-dc-view"></div>
```

[!DNL Acrobat Services]-Bibliothek einschließen:

```
  <script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

Analysieren Sie in einem benutzerdefinierten Skript-Tag den Dateinamen aus den Abfragezeichenfolgen-Parametern, damit Sie wissen, welche Datei auf der Seite eingebettet werden soll:

```
  <script type="text/javascript">
          let params = new URLSearchParams( window.location.search );
          let filename = params.get( "file" );
  </script>
```

Fügen Sie einen Dokumentereignislistener für das adobe_dc_view_sdk.ready-Ereignis hinzu, das die angegebene PDF-Datei in eine eingebettete Ansicht innerhalb des div-Elements lädt. Verwenden Sie Ihre Client-ID aus den PDF Embed-API-Zugangsberechtigungen. Sie möchten Kommentare und Anmerkungen aktivieren, also betten Sie die Ansicht in den FULL_WINDOW-Modus ein und legen Sie die Option showAnnotationsTools auf true fest.

```
  document.addEventListener( "adobe_dc_view_sdk.ready", () => { 
      var adobeDCView = new AdobeDC.View( { 
          clientId: "YOUR CLIENT ID HERE",
          divId: "adobe-dc-view",
          locale: "en-US",
      } );
      adobeDCView.previewFile( {
          content: { location: { url: "download/" + filename } },
          metaData: { fileName: "Draft Version.pdf" }
      }, {
          embedMode: "FULL_WINDOW",
          showAnnotationTools: true,
          showPageControls: true
      } );
  });
```

## Erstellen eines Benutzerprofils

Standardmäßig werden Kommentare und Anmerkungen in dieser Ansicht als &quot;Gast&quot; angezeigt. Sie können den Namen des aktuellen Reviewers für die Kommentare und Anmerkungen festlegen, indem Sie einen Benutzerprofil-Rückruf im Seitencode für die PDF-Ansicht registrieren. Im Folgenden finden Sie ein Beispielprofil. In einer vollwertigen Anwendung, die Benutzerauthentifizierung enthält, können die Profilinformationen der angemeldeten Benutzersitzung auf diese Weise festgelegt werden, um jeden Kommentator des Dokuments im Überprüfungsworkflow zu identifizieren.

```
  adobeDCView.registerCallback(
      AdobeDC.View.Enum.CallbackType.GET_USER_PROFILE_API,
      () => {
          return new Promise( ( resolve, reject ) => {
              resolve({
                  code: AdobeDC.View.Enum.ApiResponseCode.SUCCESS,
                  data: {
                      userProfile: {
                          name: "YOUR NAME",
                          firstName: "FIRST",
                          lastName: "LAST",
                          email: "document.editor@adobe.com"
                      }
                  }
              });
          });
      }
  );
```

Ihr Profil identifiziert Sie als einen bestimmten Benutzer, wenn Sie ein hochgeladenes Dokument auf dieser Webseite sehen und mit Anmerkungen versehen.

## Dokumentfeedback wird gespeichert

Nachdem ein Benutzer ein Dokument kommentiert hat, klickt er auf &quot;**Speichern&quot;.** Wenn Sie auf **Speichern** klicken, wird standardmäßig die aktualisierte PDF-Datei heruntergeladen. Ändern Sie diese Aktion, um die aktuelle PDF-Datei auf dem Server zu aktualisieren.

Fügen Sie dem Servercode einen Endpunkt `/save` hinzu, der die PDF-Datei im Ordner &quot;uploads/drafts&quot; überschreibt:

```
  // Overwrite the PDF file with latest PDF changes and annotations
  app.post( "/save", ( req, res ) => {
      if( !req.files || Object.keys( req.files ).length === 0 ) {
          return res.status( 400 ).send( "No files were uploaded." );
      }

      let file = req.files.pdf;
      file.mv( __dirname + "/uploads/drafts/" + file.name, ( err ) => {
          if( err ) {
              return res.status( 500 ).send( err );
          }
          res.send( "File uploaded" );
      });
  } );
```

Registrieren Sie einen PDF-View-Rückruf für die SAVE_API, die den Inhalt auf den /save-Endpunkt hochlädt. Sie können den Wert autoSaveFrequency ändern, damit die Anwendung Änderungen automatisch in einem Zeitgeber speichern kann und bei Abschluss zusätzliche Metadaten in die derzeit eingebettete Datei einbezieht.

```
  adobeDCView.registerCallback(
      AdobeDC.View.Enum.CallbackType.SAVE_API,
      ( metaData, content, options ) => {
          return new Promise( ( resolve, reject ) => {
              let formData = new FormData();
              formData.append( "pdf", new Blob( [ content ] ), "drafts/" + filename
  );
              fetch( "/save", {
                  method: "POST",
                  body: formData
              }).then( resp => {
                  resolve({
                      code: AdobeDC.View.Enum.ApiResponseCode.SUCCESS,
                      data: {
                          /* Updated file metadata after successful save operation */
                          metaData: Object.assign( metaData, {} )
                      }
                  });
              });
          });
      },
      {
          autoSaveFrequency: 0,
          enableFocusPolling: false,
          showSaveButton: true
      }
  );
```

Kommentare und Anmerkungen zu den Dokumententwürfen werden jetzt auf dem Server gespeichert. Sie können [mehr darüber lesen, wie Rückrufe &#x200B;](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#callbacks-workflows) in Ihren Workflow passen. [Status-Rückrufe](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#status-callback) helfen z. B. bei Dateikonflikten, wenn mehrere Personen dasselbe Dokument gleichzeitig überprüfen und kommentieren möchten.

Im letzten Schritt kombinieren Sie alle bearbeiteten Dokumente mithilfe der PDF Services API zu einer PDF-Datei.

## Zusammenführen von PDF-Dateien

Der PDF-Kombinationscode entspricht dem PDF-Erstellungscode, verwendet jedoch den Vorgang &quot;Dateien zusammenführen&quot; und fügt jede Datei als Eingabe hinzu.

```
  function combineFilesToPDF( files, outputFilename, callback ) {
      // Create an ExecutionContext using credentials and create a new operation
  instance.
      const executionContext = PDFToolsSdk.ExecutionContext.create( credentials ),
          combineFilesOperation = PDFToolsSdk.CombineFiles.Operation.createNew();

      // Set operation inputs from source files.
      files.forEach( file => {
          const input = PDFToolsSdk.FileRef.createFromLocalFile( file );
          combineFilesOperation.addInput( input );
      } );

      // Execute the operation and Save the result to the specified location.
      combineFilesOperation.execute( executionContext )
          .then( result => {
              result.saveAsFile( outputFilename );
              callback( outputFilename );
          } );
 }
```

## Die endgültige PDF herunterladen

Fügen Sie einen Endpunkt mit dem Namen &quot;/finalize&quot; hinzu, der die Funktion aufruft, um alle PDF-Dateien im Ordner &quot;`uploads/drafts`&quot; zu einer Datei &quot;`Final.pdf`&quot; zusammenzuführen, und laden Sie sie dann herunter.

```
  app.get( "/finalize", ( req, res ) => {
      fs.readdir( __dirname + "/uploads/drafts/", ( err, files ) => {
          if( err ) {
              return res.status( 500 ).send( err );
          }
          combineFilesToPDF(
              files.filter( f => f.endsWith( ".pdf" ) ).map( f => __dirname + 
  "/uploads/drafts/" + f ),
              __dirname + "/uploads/Final.pdf", ( file ) => {
              res.download( file );
          } );
      } );
  } );
```

Zum Schluss füge noch einen Link zur Web-Seite &quot;main index.html&quot; zu diesem Endpunkt hinzu. Über diesen Link können Benutzer das Ergebnis des Dokumentarbeitsablaufs herunterladen.

```
<a href="/finalize">Download final PDF</a>
```

![Screenshot des Downloads des endgültigen Dokuments](assets/reviews_3.png)

## Nächste Schritte

In diesem praktischen Tutorial wurde gezeigt, wie [!DNL Acrobat Services] APIs einen [Workflow für die Freigabe und Überprüfung von Dokumenten](https://developer.adobe.com/document-services/use-cases/collaboration/review-and-approval) in eine Webanwendung integrieren. Mit der Anwendung können Mitarbeiter im Homeoffice Dateien freigeben und im Team zusammenarbeiten. Das ist besonders für Mitarbeiter und Auftragnehmer hilfreich, die von zu Hause aus arbeiten.

Sie können diese Verfahren verwenden, um die Zusammenarbeit in Ihrer App zu aktivieren, oder [PDF Services Node SDK Samples](https://github.com/adobe/pdftools-node-sdk-samples) und [PDF Embed API Samples](https://github.com/adobe/pdf-embed-api-samples) auf GitHub durchsuchen, um zu erfahren, wie Sie Adobe-APIs sonst noch verwenden können.

Sind Sie bereit, die Freigabe von Dokumenten und die Überprüfung in Ihrer eigenen App zu aktivieren? Registrieren Sie sich bei Ihrem [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)-Entwicklerkonto. Greifen Sie kostenlos auf Adobe PDF Embed zu und testen Sie die anderen APIs sechs Monate lang kostenlos. Nach Ablauf Ihres Probe-Abos können Sie [umlagepflichtig](https://developer.adobe.com/document-services/pricing/main) für nur \$0,05 pro Dokumenttransaktion, wenn Ihr Unternehmen wächst.
