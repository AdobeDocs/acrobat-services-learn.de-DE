---
title: Verwenden der Adobe PDF Services-API zum OCR-PDF von Dateien
description: Mit OCR (Optical Character Recognition) können Sie gescannte PDF entsperren, um Text zu extrahieren und durchsuchbare Dateien zu erstellen
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6677
thumbnail: KT-6677.jpg
exl-id: 61a9a2d1-94c3-41c2-8f90-a56a938ef245
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# Verwenden der Adobe PDF Services-API zum OCR-PDF von Dateien

![PDF-Hero-Image erstellen](assets/OCR_hero.jpg)

Mit OCR (Optical Character Recognition) können Sie gescannte PDF entsperren, um Text zu extrahieren und durchsuchbare Dateien zu erstellen. Mit unseren leistungsstarken Cloud-basierten APIs lässt sich OCR in jeden Dokumenten-Workflow integrieren. Das ist die perfekte Lösung für die Archivierung, das Kopieren von Text und die Erstellung durchsuchbarer Dokumentindizes. Erstellen Sie durchsuchbare Archive aus gescannten PDF-Repositorys, um wichtige Informationen freizugeben und Zeit mit schneller Durchsuchbarkeit zu sparen. Oder wende OCR auf PDF von hochgeladenen Scans an, um sie zur Verwendung in Onboarding-Workflows bearbeiten zu können.

Entwickler können mit den für OCR bereitgestellten Beispieldateien in wenigen Minuten sofort loslegen.

In diesem Tutorial lernen Sie die Grundlagen zur Ausführung Ihres ersten PDF Services API OCR-Vorgangs mit Beispieldateien für die Sprachen Node.js, Java und .Net kennen.

## Schritt 1: Erstellen Sie Ihre Anmeldeinformationen und richten Sie Ihre Umgebung ein

Verwenden Sie die folgenden Tutorials zu ersten Schritten, um Ihre API-Zugangsberechtigungen zu erstellen, Beispieldateien herunterzuladen und Ihre Umgebung einzurichten.

[Erste Schritte mit PDF Services API und Java](gettingstartedjava.md)

[Erste Schritte mit PDF Services API und .NET](gettingstartednet.md)

[Erste Schritte mit PDF Services API und Node.js](createpdffromhtml.md)

## Führen Sie das in den Beispieldateien bereitgestellte OCR-Beispiel aus

Der OCR-Vorgang ermöglicht standardmäßig das englische Gebietsschema, bietet aber auch Unterstützung für Deutsch, Französisch, Dänisch und [andere Sprachen](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#ocr-with-explicit-language). Die Standardeinstellung ist das en-us-Gebietsschema.

Wenn Sie Optionen mit OCR-Vorgang einschließlich eines bestimmten Gebietsschemas übergeben, akzeptiert die Methode auch den Parameter &quot;type&quot;, der zwei Optionen enthält:

* SEARCHABLE_IMAGE: Ändert das Originalbild während des Bereinigungsvorgangs (z. B. zum Verzerren), bevor eine unsichtbare Textebene darüber platziert wird. Dieser Typ entfernt unerwünschte Artefakte und kann in einigen Szenarien zu einem besser lesbaren Dokument führen.

* SEARCHABLE_IMAGE_EXACT: Stellt sicher, dass Text durchsucht und ausgewählt werden kann. Diese Option behält das Originalbild bei und platziert eine unsichtbare Textebene darüber. Empfohlen für Fälle, in denen eine maximale Originaltreue erforderlich ist.

**Java**

1. Öffnen Sie eine Eingabeaufforderung.

1. Wechseln Sie in das Beispielcodeverzeichnis.

   Beispiel: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples>.

1. Führen Sie den folgenden Befehl aus:

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.ocrpdf.OcrPDF`

Die PDF wird im Verzeichnis &quot;src/main/resources&quot; erstellt.

**.Net**

1. Öffnen Sie eine Eingabeaufforderung.

1. Wechseln Sie in das Beispielcodeverzeichnis.

   Beispiel: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. Wechseln Sie die Verzeichnisse erneut in das OcrPDF-Verzeichnis.

1. Führen Sie den folgenden Befehl aus:

   `dotnet run OcrPDF.csproj`

Die PDF wird im selben Verzeichnis erstellt.

**Node.js**

1. Öffnen Sie eine Eingabeaufforderung.

1. Wechseln Sie in das Beispielcodeverzeichnis.

   Beispiel: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. Führen Sie den folgenden Befehl aus:

   `node src/ocr/ocr-pdf.js`

Die PDF wird an dem in der Ausgabe angegebenen Speicherort erstellt, bei dem es sich standardmäßig um das Ausgabeverzeichnis handelt.

## Abschließende Überlegungen

Für diese einfachen Schritte mit den Beispieldateien sollten Sie ein funktionierendes Beispiel verwenden, auf dem Sie aufbauen können. Zusätzlich zu dem OCR-Beispiel, das wir in diesem Tutorial verwendet haben, gibt es ein weiteres Beispiel für OCR mit den unterstützten Typ- und Gebietsschemaoptionen, die zuvor erläutert wurden.

Von hier aus können Sie einfach Ihre Eingabe- und Ausgabedateien im Beispiel ersetzen, um Ihre eigene PDF zu verwenden und Ihren Machbarkeitsnachweis für Ihren eigenen Anwendungsfall fertigzustellen.

![Konzeptnachweis](assets/OCR_poc.png)

## Ressourcen und nächste Schritte

* Weitere Hilfe und Unterstützung finden Sie im Adobe [[!DNL Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all)-Community-Forum

* PDF Services-API [Dokumentation](https://www.adobe.com/go/pdftoolsapi_doc)

* [Häufige Fragen](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197) zu PDF Services-API-Fragen

* [Wenden Sie sich an uns](https://www.adobe.com/go/pdftoolsapi_requestform), wenn Sie Fragen zur Lizenzierung und zu den Preisen haben
