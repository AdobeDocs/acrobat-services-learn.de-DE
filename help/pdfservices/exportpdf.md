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
TQID: https://experienceleague.adobe.com/CV-KH0fg1Tjnr7eCmNaFMtttWO1QUCyjfyIfwg1Vqm0
product_v2:
  - id: acdc2bde-2937-4877-90d9-031dd66278c9
feature_v2:
  - id: b1809bd0-a86b-4991-8083-2e3b517fc3b8
  - id: c4d07275-6387-4756-8bf7-681e581ffd27
subfeature_v2:
  - id: c6f72a9c-54c4-4933-93c9-d7c656ff1f14
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 0110d2606056220c4236fe2f0e3afbfc112746e7
workflow-type: tm+mt
source-wordcount: 524
ht-degree: 2%

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

## Schritt 2: PDF-Export-Vorgang mit den Beispieldateien ausführen

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

* Weitere Hilfe und Unterstützung finden Sie im [[!DNL Adobe Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&sort=latest_replies&filter=all)-Community-Forum

* PDF Services-API [Dokumentation](https://www.adobe.com/go/pdftoolsapi_doc)

* [Häufige Fragen](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197) zu PDF Services-API-Fragen

* [Wenden Sie sich an uns](https://www.adobe.com/go/pdftoolsapi_requestform), wenn Sie Fragen zur Lizenzierung und zu den Preisen haben
