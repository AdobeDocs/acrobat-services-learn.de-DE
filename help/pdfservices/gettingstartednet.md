---
title: Erste Schritte mit Adobe PDF Services API und .NET
description: Entwickler können in nur wenigen Minuten mit der Ausführung von Beispieldateien beginnen, die für den Zugriff auf alle verfügbaren Webdienste bereitgestellt werden.
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6675
thumbnail: KT-6675.jpg
keywords: Empfohlen
exl-id: 22c59c75-fd99-4467-a6f6-917fb246469a
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 0%

---

# Erste Schritte mit Adobe PDF Services API und .Net

![PDF Hero Image erstellen](assets/GettingStartedJava_hero.jpg)

Entwickler können in nur wenigen Minuten mit der Ausführung von Beispieldateien beginnen, die für den Zugriff auf alle verfügbaren Webdienste bereitgestellt werden. Dieses Tutorial führt Sie durch alle Schritte zum Starten der Ausführung der Beispiele mit dem PDF Services .Net SDK:

## Schritt 1: Abrufen von Anmeldedaten und Herunterladen von Beispieldateien

Der erste Schritt besteht darin, eine Zugangsberechtigung (API-Schlüssel) zu erhalten, um die Verwendung zu entsperren. [Für die kostenlose Testversion hier anmelden](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) und klicken Sie auf &quot;Jetzt loslegen&quot;, um Ihre neuen Anmeldeinformationen zu erstellen.

![Schritt 1](assets/GettingStartedJava_step1.png)

Es ist wichtig, ein &quot;persönliches Konto&quot; auszuwählen, um sich für die kostenlose Testversion anzumelden:

![Persönlich](assets/GettingStartedJava_personal.png)

Im nächsten Schritt wählen Sie den PDF Services API-Dienst aus und fügen dann einen Namen und eine Beschreibung für Ihre Anmeldeinformationen hinzu.

Es gibt ein Kontrollkästchen für &quot;Personalisiertes Codebeispiel erstellen&quot;. Wählen Sie diese Option, wenn Ihre neuen Anmeldeinformationen automatisch zu Ihren Beispieldateien hinzugefügt werden sollen. Dadurch sparen Sie den manuellen Schritt des Hinzufügens zu Ihrem Projekt.

Wählen Sie als Nächstes &quot;Node.js&quot; als Ihre Sprache aus, um die spezifischen &quot;Node.js&quot;-Beispiele zu erhalten, und klicken Sie auf die Schaltfläche &quot;Credentials erstellen&quot;.

![Anmeldedaten](assets/GettingStartedJava_credentials.png)

Sie erhalten eine ZIP-Datei mit dem Namen PDFToolsSDK-.NetSamples.zip, die Sie herunterladen und in Ihrem lokalen Dateisystem speichern können.

## Schritt 2: .NET-Umgebung einrichten und Beispielcode ausführen

1. Laden Sie das [.NET SDK](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/install)
1. Extrahieren Sie die heruntergeladene **[!UICONTROL PDFToolsSDK-.NetSamples.zip]** und entpacken Sie den Inhalt
1. cd zum Stammverzeichnis der Beispiele **[!UICONTROL adobe-DC.PDFTools.SDK.NET.Samples]**
1. Führen Sie im Stammverzeichnis der Beispiele `dotnet build`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>dotnet build

   Jetzt können Sie die Beispieldateien ausführen.

   Die folgenden letzten Schritte zeigen Ihnen, wie Sie Ihr erstes Beispiel mit dem Vorgang &quot;PDF aus Word erstellen&quot; ausführen:

1. Wechseln Sie im Stammverzeichnis der Beispiele zum Ordner CreatePDFFromDocx, cd CreatePDFFromDocx/

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>cd CreatePDFFromDocx/

1. Ansturm `dotnet run CreatePDFFromDocx.csproj`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples\CreatePDFFromDocx>dotnet run CreatePDFFromDocx.csproj

Ihre PDF wird an dem in der Ausgabe angegebenen Speicherort erstellt, der standardmäßig derselbe Ordner ist.

## Abschließende Überlegungen

Die PDF Services API hilft Ihnen, manuelle Prozesse zu vermeiden, indem gängige Workflows automatisiert und der Verarbeitungsaufwand auf die Cloud verlagert wird. In einer Welt, in der jeder Browser das PDF anders behandelt, können Sie mithilfe der Adobe PDF Embed-API und der PDF Services-API optimierte, zuverlässige und vorhersehbare Prozesse erstellen, die korrekt ausgeführt werden und angezeigt werden **jedes Mal** unabhängig von der Plattform oder dem Gerät.

## Ressourcen und nächste Schritte

* Weitere Hilfe und Unterstützung finden Sie auf der [[!DNL Adobe Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) Community-Forum

* PDF Services-API [Dokumentation](https://www.adobe.com/go/pdftoolsapi_doc)

* [FAQ](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) für PDF Services API-Fragen

* [Kontakt](https://www.adobe.com/go/pdftoolsapi_requestform) bei Fragen zur Lizenzierung und Preisgestaltung

* Verwandte Artikel

  [Die neue PDF Services-API bietet noch mehr Funktionen für Dokumenten-Workflows](https://community.adobe.com/t5/document-services-apis/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [Juli-Version von [!DNL Adobe Acrobat Services]: PDF-Einbettungs- und PDF-Dienste](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
