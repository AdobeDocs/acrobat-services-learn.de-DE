---
title: Verwalten von Workflows für Finanzdokumente in Java
description: "[!DNL Adobe Acrobat Services] bietet alle erforderlichen Tools, Services und Funktionen, um Daten aus PDF-Finanzdokumenten zu verarbeiten und zu extrahieren."
type: Tutorial
role: Developer
level: Intermediate
feature: Use Cases
thumbnail: KT-7482.jpg
jria: KT-7482
exl-id: 3bdc2610-d497-4a54-afc0-8b8baa234960
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 0%

---

# Verwalten von Workflows für Finanzdokumente auf Java

![Use Case Hero-Banner](assets/UseCaseFinancialHero.jpg)

In der Finanzbranche werden PDF-Dateien häufig für den Datenaustausch eingesetzt, da Format, Design und Struktur von Dokumenten dadurch beibehalten werden. Dieses robuste Format ermöglicht es Finanzanalysten und Beratern, ihren Kunden bei fundierten Entscheidungen zu helfen.

Die Verarbeitung und Automatisierung des PDF-Formats kann sich als Herausforderung erweisen, insbesondere wenn mehrere Datenquellen kombiniert werden müssen - ein gängiges Anwendungsszenario in der Finanzbranche. Die Entwicklung einer maßgeschneiderten Lösung zur Verarbeitung von PDF-Dokumenten ist eine Option, aber es besteht keine Notwendigkeit, zu viel Zeit und Geld in Software und Infrastruktur zu investieren. [!DNL Adobe Acrobat Services] stellt alle erforderlichen Tools, Services und Funktionen bereit, um Daten aus PDF-Dokumenten zu verarbeiten und zu extrahieren.

## Lernziel.

In diesem praktischen Tutorial erfahren Sie, wie Sie [!DNL Adobe Acrobat Services] APIs für [!DNL Java Spring Boot] Anwendungen. Sie erstellen eine MVC-App (Model-View-Controller), die Inhalte aus PDF-Dokumenten extrahiert, in andere Datenformate wie Excel konvertiert, mehrere PDF kombiniert und die Ressourcen mit einem Kennwort schützt. In diesem Tutorial wird erläutert, wie Sie PDF-Dokumente verarbeiten und mithilfe der Adobe auf Ihren Websites anzeigen. [PDF Embed-API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

## Relevante APIs und Ressourcen

* [PDF Services-API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF Embed-API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Projektbeispiele](https://github.com/adobe/pdftools-java-sdk-samples)

## Einrichten

[!DNL Adobe Acrobat Services] verwendet ein Authentifizierungssystem, um den Ressourcenzugriff zu steuern. Um auf die Services zugreifen zu können, müssen Sie in der Adobe einen API-Schlüssel für Ihr Unternehmen oder Ihre Anwendung anfordern. Wenn Sie über einen API-Schlüssel verfügen, fahren Sie mit dem nächsten Abschnitt fort. Um einen neuen API-Schlüssel zu erstellen, besuchen Sie [Erste Schritte](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) in der &quot; [!DNL Acrobat Services] Website. Sie können einen Schlüssel mit ihrer kostenlosen Testversion erstellen, die 1.000 Dokumenttransaktionen bietet, die bis zu sechs Monate lang genutzt werden können.

Um dieses Tutorial nachzuvollziehen, benötigen Sie zwei Sätze von API-Schlüsseln:

* Adobe PDF Services - zur Verarbeitung des PDF-Dokuments

* Adobe PDF Embed-API

Kopieren Sie nach dem Erstellen der Anmeldeinformationen die API-Anmeldeinformationen für PDF Services und den privaten Schlüssel in die Datei [!DNL Spring Boot] im Abschnitt &quot;Ressourcen&quot;. Erfahren Sie mehr über die [Maven- und Gradle-Bibliotheken und -Abhängigkeiten](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=services) im Fenster &quot; [!DNL Adobe Acrobat Services] Website. Stellen Sie sicher, dass Sie alle erforderlichen Pakete und Bibliotheken eingerichtet haben, bevor Sie fortfahren.

![Screenshot des Verzeichnisspeicherorts für PDF Services-API-Zugangsberechtigungen](assets/FAWJ_1.png)

Informationen zum Konfigurieren der Protokollierungsdienste finden Sie unter [Dokumentation zur Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=services) und scrollen Sie zum Abschnitt Protokollierung .

>[!NOTE]
>
> Speichern Sie in Ihrer Produktionsumgebung nicht die privaten Schlüssel in der Versionskontrolle. Verwenden Sie immer einen geheimen Tresor oder einen Schlüsselinjektionsdienst, um die unbefugte Verwendung von Anmeldeinformationen zu verhindern.

Jetzt, da Ihr [!DNL Spring Boot] konfiguriert ist, können Sie mit der Verarbeitung der PDF und der Generierung von Berichten für Kunden fortfahren.

## Übermitteln der Berichtsdaten

Um die Adobe PDF Services-API zu verwenden, richten Sie zunächst ein `ExecutionContext` , der die von Ihnen angegebenen Anmeldeinformationen verwendet. Da Sie die Anmeldeinformationen in Ihrer Anwendung haben, können Sie sie aus der Datei lesen und den Kontext wie folgt erstellen:

```
Credentials credentials = Credentials.serviceAccountCredentialsBuilder()
    .fromFile(AUTH_FILE_PATH)
    .build();

ExecutionContext executionContext = ExecutionContext.create(credentials);
```

Rufen Sie als Nächstes den Kontext zur Verarbeitung der PDF-Dokumente ab. Hier sind die Aktionen, die Sie ausführen können:

* PDF-Dokumente (in Excel-, Word- oder Grafikdateien) konvertieren

* Erstellen Sie die PDF-Dokumente (von HTML, Excel, Word und mehr)

* Mehrere PDF-Dokumente kombinieren

* Protect verwenden und den Schutz der PDF-Dokumente aufheben (Kennwort erforderlich)

* Optimieren der PDF von Dokumenten für die Bereitstellung in Netzwerken

Alle diese Beispiele finden Sie im [GitHub-Beispiele](https://github.com/adobe/pdfservices-java-sdk-samples/tree/master/src/main/java/com/adobe/pdfservices/operation/samples) Projektarchiv.

Als Nächstes in [!DNL Spring Boot]können Sie eine Datei mithilfe des String-Pfads oder des Streams abrufen, in den die Datei hochgeladen wird. Jeder ausgeführte Vorgang muss initialisiert und ein Pfad für die Eingabedatei festgelegt werden. Für dieses Tutorial verwenden Sie die öffentlich verfügbaren PDF-Berichte von [Blackrock](https://www.blackrock.com/us/individual/products/investment-funds). Sie können jede andere Quelle verwenden, einschließlich Ihrer eigenen Berichte.

Beginnen Sie mit der Aufnahme des [FileRef](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/io/FileRef.html) -Objekt aus der Datei. Der Einfachheit halber sollten Sie sich auf die Dateien konzentrieren, indem Sie den Pfad &quot;Zeichenfolge&quot; eingeben. Im Folgenden erstellen Sie einen Vorgang zum Konvertieren einer Datei in Ihrem Pfad von PDF in Excel:

```
ExecutionContext executionContext = ExecutionContext.create(credentials);
ExportPDFOperation exportOperation = ExportPDFOperation.createNew(ExportPDFTargetFormat.XLSX);

// Create the input source
FileRef inputPdf = FileRef.createFromLocalFile(INPUT_PDF);
exportOperation.setInput(inputPdf);
```

Danach ist das Programm bereit, den ersten Vorgang auf der PDF auszuführen. Als Nächstes führen Sie den Vorgang aus und erhalten das Ergebnis in der Excel-Tabelle:

```
try {
    FileRef output = exportOperation.execute(executionContext);
    output.saveAs(OUTPUT_EXCEL);
} catch (ServiceApiException e) {
    e.printStackTrace();
}
```

In diesem Szenario wird nur eine PDF-Datei verarbeitet. Sie können auch mit mehreren PDF-Dateien beginnen und sie zu einer einzigen Datei zusammenführen. Die Verwendung mehrerer Dateien ist in der Finanzdatenberichterstattung üblich, da Sie Mittel aus mehreren Quellen verarbeiten müssen, um einen umfassenden Bericht zu erstellen.

## Bericht generieren

[!DNL Adobe Acrobat Services] Die vorkonfigurierte Verarbeitung von Excel-Dokumenten wird nicht unterstützt, Sie können jedoch weiterhin Community-Frameworks und Bibliotheken verwenden, um den Inhalt zu verarbeiten.

Sie können z. B. den Katalog [Apache POI](https://poi.apache.org/) , um Excel (oder andere Microsoft-Dokumente) in Ihrem [!DNL Java Spring Boot] -App, oder Sie können andere manuelle oder automatisierte Aufgaben an der Excel-Datei ausführen.

In diesem Beispiel beginnend mit Ihren PDF-Dokumenten extrahieren Sie den Nettoinventarwert Ihrer drei Fonds und zeigen ihn in einer Tabelle an. Sie können auch andere Informationen abrufen, wie Diagramme und Tabellen, basierend auf Ihren Anforderungen und den verfügbaren Daten. Ihr könnt sogar Daten aus anderen Quellen importieren.

Nachdem der Bericht generiert wurde (in diesem Beispiel im Excel-Format), können Sie den Bericht mithilfe von Adobe PDF Services-Vorgängen wieder in ein PDF-Dokument konvertieren und schützen.

Gehen Sie wie folgt vor, um den Bericht vom Excel-Format in ein PDF-Dokument zu konvertieren:

```
ExecutionContext executionContext = ExecutionContext.create(credentials);
CreatePDFOperation exportOperation = CreatePDFOperation.createNew();

// Create the input source
FileRef inputPdf = FileRef.createFromLocalFile(INPUT_EXCEL);
exportOperation.setInput(inputPdf);

try {
    FileRef output = exportOperation.execute(executionContext);
    output.saveAs(OUTPUT_PDF);
} catch (ServiceApiException e) {
    e.printStackTrace();
}
```

>[!TIP]
>
> Um zu verhindern, dass das Objekt jedes Mal neu erstellt werden muss, wenn eine Anforderung eingeht, verwenden Sie die Abhängigkeitsinjektion von Spring, um die `ExecutionContext` Objekt.

Dieser Code generiert aus dem Bericht ein PDF-Dokument im Excel-Format.

Bevor Sie diese PDF an Ihre Kunden senden, können Sie sie mit einem Kennwort schützen. Erstellen Sie einen weiteren Vorgang, der diesen Schutz für Sie behandelt. [ProtectPDFOperation](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/pdfops/ProtectPDFOperation.html), verwenden Sie dann [ProtectPDFOptions](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/pdfops/options/protectpdf/package-summary.html) , um das Kennwort zum Dokument hinzuzufügen.

```
ProtectPDFOptions options = ProtectPDFOptions.passwordProtectOptionsBuilder()
                    .setUserPassword("p@55w0rd")
                    .setEncryptionAlgorithm(EncryptionAlgorithm.AES_256)
                    .build();
ProtectPDFOperation operation = ProtectPDFOperation.createNew(options);
```

Geben Sie als Nächstes die Eingabe an und führen Sie den Vorgang aus. Die resultierende Datei sollte mit einem Kennwort versehen sein, um unbefugten Zugriff zu verhindern.

## Anzeigen des Berichts

Nachdem der PDF-Bericht erstellt wurde, können Sie ihn über die Adobe PDF Embed-API auf der Website anzeigen. Mit dieser JavaScript-API können Webentwickler die PDF-Dokumente nativ im Webbrowser laden und rendern.

>[!NOTE]
>
> An dieser Stelle benötigen Sie das zweite Anmeldeinformationstoken, die Client-ID.

In Ihrem [!DNL Spring Boot] &quot; das folgende HTML-Snippet hinzu, in dem Sie den PDF-Bericht rendern möchten:

```
<div id="pdf-viewer"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
    document.addEventListener("adobe_dc_view_sdk.ready", function()
    {
        var adobeDCView = new AdobeDC.View({ clientId: "<your-client-id-here>", divId: "pdf-viewer" });
        adobeDCView.previewFile(
        {
            content: {
                location: {
                    url: "<your-document.pdf>"
                }
            },
            metaData: {
                fileName: "<document-name.pdf>"
            }
        });
    });
</script>
```

Dieses Skript lädt das PDF-Dokument und ermöglicht es den Betrachtern, die Dokumente mit Anmerkungen zu versehen und zu kommentieren. Hier ist die Ansicht dieser Einbettungs-API, wie in Firefox gezeigt:

![Screenshot eines PDF-Dokuments in Firefox](assets/FAWJ_2.png)

Die PDF Embed-API bietet alle erforderlichen Tools, um die PDF in der Vorschau anzuzeigen und Anmerkungen zum Bericht hinzuzufügen.

## Nächste Schritte

In diesem praktischen Tutorial erfährst du, [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) APIs und erörterten, wie diese Services verwendet werden können, um PDF-Daten zu verarbeiten und Berichte für finanzielle Entscheidungen zu generieren. Es wurde gezeigt, wie Sie die APIs mithilfe von [!DNL Java Spring Boot] als Beispiel-Framework, um zu zeigen, wie einfach die schnelle Verarbeitung von PDF-Dokumenten ist.

Entdecken [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) und erfahrt, wie Adobe PDF Services eurem Unternehmen helfen kann. Weitere Informationen zu den im SDK verfügbaren Funktionen finden Sie im [GitHub-Repository](https://github.com/adobe/pdftools-java-sdk-samples) für die Beispiele auswählen und untersuchen, wie [PDF Embed-API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) können Ihnen dabei helfen, PDF schnell in Ihren Anwendungen zu zeigen.

Um Dokumente problemlos kombinieren und bearbeiten zu können und hilfreiche PDF-Berichte für Ihre Finanzkunden zu erstellen, melden Sie sich zunächst kostenlos an [Adobe-Entwicklerkonto](https://www.adobe.io/apis/documentcloud/dcsdk/) noch heute.
