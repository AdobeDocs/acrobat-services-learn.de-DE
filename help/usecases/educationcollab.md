---
title: Zusammenarbeit von Schülern, Studierenden, Lehrkräften
description: Erfahren Sie, wie Sie eine Online-Lernplattform erstellen, mit der Lehrkräfte, Schüler und Studierende Ressourcen in PDF gemeinsam nutzen können.
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8091
thumbnail: KT-8091.jpg
exl-id: 570a635c-e539-4afc-a475-ecf576415217
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '1485'
ht-degree: 0%

---

# Zusammenarbeit von Schülern, Studierenden, Lehrkräften

![Use Case Hero-Banner](assets/UseCaseStudentHero.jpg)

Bildungseinrichtungen verwenden PDF-Dokumente, um Lernmaterialien für Schüler und Studierende freizugeben. PDF bieten ein austauschbares Dokumentformat für Lehrkräfte.

Integrieren [Adobe PDF Services API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html) und [Adobe PDF Embed-API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) bietet Lehrkräften, Schülern und Studierenden eine zentrale Plattform zum Unterrichten und Lernen. Beispielsweise kann Ihre App Schülern und Studierenden ermöglichen, Fragen zu ihren Aufgaben und Berichtskarten zu stellen und an Gruppenaufgaben zusammenzuarbeiten.

Es gibt ein offizielles SDK für Node.js-Anwendungen, um auf die PDF Services API zuzugreifen. Damit kannst du Dokumente wie Microsoft Word oder Microsoft Excel in PDF umwandeln. Sie können auch komplexere Vorgänge durchführen, z. B. mehrere Berichte kombinieren, Seiten neu anordnen und PDF schützen. Weitere Informationen finden Sie unter [Produktdokumentation](https://www.adobe.io/apis/documentcloud/dcsdk/).

## Lernziel.

In diesem praktischen Tutorial lernen Sie, wie Sie eine Online-Lernplattform erstellen, die [Schüler, Studierende und Lehrkräfte erhalten die Möglichkeit, Ressourcen](https://www.adobe.io/apis/documentcloud/dcsdk/student-teacher-collaboration.html) auf PDF. In diesem Tutorial wird ein [Lernportal](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers) erstellt mit der JavaScript-Laufzeitumgebung Node.js (Node.js) und den PDF-Diensten.

Das Lernportal verfügt über die folgenden Funktionen:

* Lehrkräfte können Ressourcen hochladen.

* Schüler/Studierende können mehrere Dokumente auswählen, die auf einen PDF konvertiert werden sollen

* Ermöglicht die Konvertierung von Dokumenten zum PDF

* Bietet eine PDF-Vorschau für Schüler, Studierende und Azubis in einem Webbrowser und ermöglicht ihnen das Kommentieren der Dokumente ohne zusätzliche Software.

* Schüler/Studierende können Kommentare hinterlassen und auf ihren Computer herunterladen

Weitere Infos [!DNL Adobe Acrobat Services] bieten ein umfassendes Lernerlebnis für Schüler, Studierende und PDF. [!DNL Acrobat Services] APIs lassen sich nahtlos in vorhandene Programme integrieren, sodass Schüler, Studierende und Azubis Dateien hochladen, konvertieren und anzeigen sowie Kommentare dazu abgeben und speichern können - alles im aktuellen Setup.

## Relevante APIs und Ressourcen

* [PDF Embed-API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [PDF Services-API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Projektcode](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers)

## Hochladen von Ressourcen auf das Lernportal

Im Bereich &quot;Lehrkräfte&quot; des Lernportals können Lehrkräfte Dokumente wie Aufgaben und Tests hochladen. Die Dokumente können in jedem Format vorliegen, z. B. Microsoft Word, Microsoft Excel, HTML, verschiedene Bildformate usw.

![Screenshot des Lehrerbereichs des Lernportals](assets/edu_1.png)

Hochgeladene Dokumente werden gespeichert und den Schülern präsentiert, wenn sie ihre Webseite öffnen.

Weitere Informationen dazu, wie die Anwendung die Dateien hochlädt, finden Sie in den [Projektschlüssel](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers).

## Konvertieren von Dokumenten in PDF

Schüler können ein oder mehrere Dokumente eines beliebigen Typs in PDF, z. B. Microsoft Word, Excel und PowerPoint, sowie andere gängige Text- und Bilddateiformate konvertieren. Das Lernportal verwendet PDF Services, um Dateien in PDF zu konvertieren.

Um ein eigenes Lernportal zu erstellen, müssen Sie zunächst Ihre eigenen Anmeldeinformationen erstellen. [Anmelden](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) um die PDF Services API sechs Monate lang kostenlos und bis zu 1.000 Dokumenttransaktionen zu nutzen. Danach [pay as you go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) bei nur \$0.05 pro Dokumenttransaktion, wenn die Klasse ihre Aufgaben beschleunigt.

Wenn ein Schüler/Studierender ein Dokument im Dashboard auswählt, wird Folgendes angezeigt:

![Screenshot des Abschnitts &quot;Schüler&quot; des Lernportals](assets/edu_2.png)

Der Schüler wählt einfach die zu konvertierenden Dokumente aus und klickt auf **Report abrufen**.

Das Lernportal konvertiert die Dokumente in PDF und zeigt eine Berichtsseite zusammen mit einer Vorschau der PDF-Datei an.

Hier ist der Beispielcode für diesen Schritt:

```
async function createPdf(rawFile, outputPdf) {
    try {
            // configurations
            const credentials =  adobe.Credentials
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

Der Beispielcode ruft die Datei `createPdf` -Methode im Express-Routenhandler, um die PDF zu generieren.

Informationen dazu, wie diese Methode aufgerufen wird, finden Sie unter [den Projektcode](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers/blob/master/src/helpers/pdf.js).

## Vorschau der Lernressourcen

Die Benutzeroberfläche verwendet die PDF Embed-API zum Rendern von PDF in einem Webbrowser. Diese API kann kostenlos verwendet werden.

Die PDF Embed-API verwendet andere Anmeldedaten als die PDF Services-API. Sie müssen also [Anmeldeinformationen erstellen](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
bevor du es benutzen kannst. Dann können Sie PDF Embed völlig kostenlos verwenden.

Stellen Sie sicher, dass Sie die richtige Website-URL in das Token eingeben. Andernfalls können Sie die PDF möglicherweise nicht mit dem Token rendern.

Die Benutzeroberfläche verwendet den Katalog [Lenker](https://handlebarsjs.com/) Vorlagensprache. Die PDF wird in einem Webbrowser angezeigt.

Hier ist der Code für diesen Schritt:

```
<div id="adobe-dc-view" style="height: 750px; width: 700px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
    document.addEventListener("adobe_dc_view_sdk.ready", function () {
        var adobeDCView = new AdobeDC.View({ clientId: "<your-credentials-here>", divId: "adobe-dc-view" });
        adobeDCView.previewFile(
            {
                content: {
                    location: { url: "<file-url>" }
                },
                    metaData: { fileName: "<file-name>" }
            },
           );
    });
</script>
 
<p>Material has been generated, <a href="/students/download/{{filename}}" target="_blank">click here</a> to download it.
</p>
```

Dieser Code zeigt die PDF-Ausgabe und den Link zum Herunterladen des PDF-Berichts an, wie in der folgenden Bildschirmaufnahme gezeigt:

![Screenshot der PDF-Vorschau für Schüler](assets/edu_3.png)

Schüler sollten den Bericht herunterladen oder hier an dem Material arbeiten können.

## Kommentieren von PDF-Dokumenten

Eine Lernplattform sollte grundlegende Anmerkungen, Kommentare und Diskussionen in PDF unterstützen. Die PDF Embed-API bietet all diese Funktionen. Aktiviert die Unterstützung von Anmerkungen mit `showAnnotationTools`, sodass Lehrkräfte und Schüler die Dokumente kommentieren und Kommentare als Teil der PDF archivieren können.

Um Anmerkungen in PDF-Dokumenten zu aktivieren, müssen Sie nur das Argument übergeben `showAnnotationTools` : true, um die `previewFile` -Methode. Dadurch wird das Anmerkungswerkzeug in der PDF-Vorschau angezeigt. Rufen Sie dieses Werkzeug über das Menü mit den drei Punkten in der oberen rechten Ecke der Vorschau auf.

![Screenshot der Kommentarwerkzeuge auf dem PDF](assets/edu_4.png)

In den von den Lehrkräften hochgeladenen Dokumenten können die Schüler Text hervorheben, Kommentare hinzufügen usw.

![Screenshot des Hinzufügens eines Kommentars in PDF](assets/edu_5.png)

In der obigen Bildschirmaufnahme wird der Benutzer als &quot;Gast&quot; bezeichnet, Sie können jedoch Profile für Benutzer wie Schüler, Studierende, Lehrkräfte und Dozenten konfigurieren.

Wenn ein Schüler/Studierender eine Anmerkung anwendet, zeigt die PDF Embed-API eine **Speichern** entlang des oberen Banners. Beim Speichern werden die Anmerkungen zur Datei hinzugefügt. Klicken **Speichern** , um zu sehen, wie die Datei mit der im Bericht eingebetteten Anmerkung gespeichert wird.

Die Schüler können Anmerkungen verwenden, um Fragen zu stellen oder ihre Kommentare zum Lernmaterial zu teilen.

## Verfolgen der Dokumentverwendung

Für Lehrkräfte und Schulen ist es wichtig zu sehen, wie Schüler Online-Plattformen nutzen. Dies hilft Lehrkräften, ihre Schüler und Studierenden mit Ressourcen zu unterstützen, die ihnen helfen, ihre Aufgaben besser zu erfüllen. Die PDF Embed-API ist mit Analysen integriert, mit denen Sie alle stattfindenden Ereignisse messen können, z. B. das Öffnen, Lesen und Schließen von Dokumenten. Mit der PDF Services API können Lehrkräfte auch das Drucken, Herunterladen und Ändern von Dateien deaktivieren, um die akademische Integrität zu wahren.

Wenn Sie ein [Adobe Analytics](https://www.adobe.io/apis/experiencecloud/analytics.html) Lizenz verwenden, können Sie ihre [Standardmäßige Integration](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfembed/controlpdfexperience.html?lang=en#adobe-analytics). Andernfalls verwenden Sie Rückrufe, um Ihre PDF-Services mit anderen Analyseanbietern wie [Google](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfembed/controlpdfexperience.html?lang=en#google-analytics).

Um die Messung von Dokumentereignissen zu aktivieren, hängen Sie die Ereignishandler mit dem Attribut `registerCallback` Methode mit der Adobe DC View-Instanz. Sie können grundlegende Kennzahlen wie das Öffnen eines Dokuments oder das Lesen einer Seite auf der Konsole anzeigen. Sie können die Metriken auch in einem Protokoll speichern oder in anderen Analyse-Stores veröffentlichen.

Hier ist der Beispielcode zum Anhängen der Ereignishandler:

```
adobeDCView.registerCallback(
    AdobeDC.View.Enum.CallbackType.EVENT_LISTENER,
    function(event) {
           console.log(event);
    },
    {
           enablePDFAnalytics: true
    }
);
```

Lehrer können sehen, wie viele Schüler den Auftrag gesehen haben, wie viele die Seiten ihrer Notizen durchlaufen haben und andere wertvolle Details sehen.

Hier ist eine Bildschirmaufnahme der Webbrowser-Konsole:

![Screenshot der Webbrowser-Konsole](assets/edu_6.png)

Diese Bildschirmaufnahme zeigt, dass der Schüler die Aufgabendatei geöffnet hat, er die erste Seite gelesen hat - er hat entweder nicht zu weiteren Seiten gescrollt oder das Dokument hatte nur eine Seite - dann hat er die Datei heruntergeladen. Ihr könnt diese Kennzahlen erfassen, um Analysen durchzuführen und das Verhalten eurer Schüler zu untersuchen.

Außerdem [Adobe Analytics](https://business.adobe.com/products/analytics/adobe-analytics.html) ist mit der PDF Embed-API integriert. Wenn Sie also über ein Adobe Analytics-Abonnement verfügen, können Sie Ihre Kennzahlen in Ihrem Abonnement veröffentlichen. Um die Metriken in Adobe Analytics zu veröffentlichen, müssen Sie lediglich Ihre Suite-ID an den PDF Embed-API-Konstruktor übergeben. (Beachten Sie, dass Sie Ihre PDF Embed-API-Anmeldedaten verwenden müssen, nicht Ihre PDF Services API-Anmeldedaten).

Hier ist ein Beispielcode, der zeigt, wie die Suite ID an den PDF Embed-API-Konstruktor übergeben wird:

```
var adobeDCView = new AdobeDC.View({
    clientId: "<your-adobe-dc-credential>",
    divId: "<#element>"
    reportSuiteId: <your-id-here>,
}); 
```

## Nächste Schritte

In diesem praktischen Tutorial wurde die Verwendung der PDF Services-API und der PDF Embed-API zum Erstellen eines Lernportals zur Erleichterung effektiver [Zusammenarbeit zwischen Schülern, Studierenden und Lehrkräften](https://www.adobe.io/apis/documentcloud/dcsdk/student-teacher-collaboration.html). Mit diesem Portal können Lehrkräfte Lernmaterial in jedem Format hochladen und es mithilfe der PDF Services API in PDF konvertieren. Die Schüler/Studierenden können diese PDF dann mithilfe der PDF Embed-API in der Vorschau anzeigen.

Nachdem Sie nun wissen, wie Sie PDF-Berichte kommentieren, die Anmerkungen archivieren und die Verwendung von PDF-Berichten verfolgen, können Sie mit der Implementierung dieser Lösungen in Ihren eigenen Projekten beginnen.

Sie können [!DNL Adobe Acrobat Services] APIs zur Erstellung benutzerfreundlicher, interaktiver PDF-Erlebnisse auf Ihrer Website. Adobe PDF Services API sechs Monate lang kostenlos nutzen - danach [pay as you go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) (über AWS oder eine Direktvereinbarung) für nur \$0.05 pro Dokumenttransaktion. Nutze Adobe PDF Embed kostenlos und unbegrenzt. Erstellen Sie ein kostenloses Konto für [Erste Schritte](https://www.adobe.com/go/dcsdks_credentials) noch heute.
