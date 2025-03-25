---
title: Verwenden der PDF Services API zum Exportieren von PDF in Word, PowerPoint und mehr
description: Erfahren Sie, wie Sie den PDF Services API-Exportvorgang mithilfe von Beispieldateien für die Sprachen Node.js, Java und .Net ausführen.
feature: PDF Services API
role: Developer
level: Intermediate
type: Tutorial
jira: KT-6674
thumbnail: KT-6674.jpg
exl-id: 55f5b04e-0249-47d9-9131-2f9ec01db7e8
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---

# Verwenden der PDF Services API zum Exportieren von PDF in Word, PowerPoint und mehr

![PDF-Hero-Image erstellen](assets/ExportPDF_hero.jpg)

Adobe PDF Services API konvertiert PDF-Dateien mithilfe von APIs in MS Office-, Text- und Bilddateien. Es gibt viele gängige Anwendungsfälle, um bestehende PDF für die Bearbeitung und Analyse von Inhalten freizuschalten, und mit PDF Services API können Entwickler diese Funktion ganz einfach in bestehende Systeme und Anwendungen integrieren. Wandle PDF-Dateien in Microsoft Word um, um Inhalte zu bearbeiten, Genehmigungen zu erteilen und später zur Unterzeichnung zu versenden. So erstellst du benutzerdefinierte Vertrags-Workflows. Oder exportieren Sie PDF-Inhalte in das MS Excel-Format, um sie für Rechnungs- und Finanzberechnungen oder Datenanalysen zu verwenden.

Der Exportvorgang unterstützt die folgenden PDF-Dateikonvertierungen:

* PDF in Microsoft Word (DOC, DOCX)
* PDF auf Microsoft PowerPoint (PPTX)
* PDF in Microsoft Excel (XLSX)
* PDF in Text (RTF)
* PDF auf Bild (JPEG, PNG)

In diesem Tutorial lernen Sie die Grundlagen zur Ausführung Ihres ersten PDF Services-API-Exportvorgangs mithilfe von Beispieldateien für die Sprachen Node.js, Java und .Net.

## Schritt 1: Erstellen Sie Ihre Anmeldeinformationen und richten Sie Ihre Umgebung ein:

Verwenden Sie die folgenden Tutorials zu ersten Schritten, um Ihre API-Zugangsberechtigungen zu erstellen, Beispieldateien herunterzuladen und Ihre Umgebung einzurichten.

[Erste Schritte mit PDF Services API und Java](gettingstartedjava.md)

[Erste Schritte mit PDF Services API und .NET](gettingstartednet.md)

[Erste Schritte mit PDF Services API und Node.js](createpdffromhtml.md)

## Schritt 2: PDF-Exportvorgang mit den Beispieldateien ausführen

**Java**

1. Öffnen Sie eine Eingabeaufforderung.

1. Wechseln Sie in das Beispielcodeverzeichnis.

   Beispiel: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples

1. Führen Sie den folgenden Befehl aus:

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.exportpdf.ExportPDFToDOCX`

Die PDF wird im Verzeichnis &quot;src/main/resources&quot; erstellt.

**.Net**

1. Öffnen Sie eine Eingabeaufforderung.

1. Wechseln Sie in das Beispielcodeverzeichnis.

   Beispiel: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. Wechseln Sie die Verzeichnisse erneut in das Verzeichnis ExportPDFtoDocx.

1. Führen Sie den folgenden Befehl aus:

   `dotnet run ExportPDFToDocx.csproj`

Die PDF wird im selben Ordner erstellt.

**Node.js**

1. Öffnen Sie eine Eingabeaufforderung.

1. Wechseln Sie in das Beispielcodeverzeichnis.

   Beispiel: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. Führen Sie den folgenden Befehl aus:

   `node src/ocr/ocr-pdf.js`

Die PDF wird an dem in der Ausgabe angegebenen Speicherort erstellt, der standardmäßig das Verzeichnis pdfServicesSdkResult ist.

## Abschließende Überlegungen

Sie sollten jetzt über ein funktionierendes Beispiel verfügen, das in Ihre vorhandenen Anwendungen importiert werden kann, um einen Machbarkeitsnachweis zu erstellen. In jedem der Beispielverzeichnisse sehen Sie ein weiteres Beispiel für den Export von PDF-Dateien in das Bildformat. Mit den gleichen Schritten oben können Sie auch dieses Beispiel ausführen. Um zu einem anderen Format zu wechseln, können Sie den Code wie folgt auf das neue Format aktualisieren:

SupportedTargetFormats.PPTX

Und das Zielergebnis:

output/exportPdfOutput.PPTX

in ein anderes Format konvertieren.

## Ressourcen und nächste Schritte

* Weitere Hilfe und Unterstützung finden Sie im [[!DNL Adobe Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all)-Community-Forum

* PDF Services-API [Dokumentation](https://www.adobe.com/go/pdftoolsapi_doc)

* [Häufige Fragen](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197) zu PDF Services-API-Fragen

* [Wenden Sie sich an uns](https://www.adobe.com/go/pdftoolsapi_requestform), wenn Sie Fragen zur Lizenzierung und zu den Preisen haben
