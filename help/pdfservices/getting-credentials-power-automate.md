---
title: Abrufen von Anmeldedaten für Microsoft Power Automate
description: Hier erfahren Sie, wie Sie Anmeldeinformationen abrufen, um Adobe PDF Services zu verwenden oder zu testen.
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-10382
thumbnail: KT-10382.jpg
exl-id: 68ec654f-74aa-41b7-9103-44df13402032
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '872'
ht-degree: 1%

---

# Abrufen von Anmeldedaten für Microsoft Power Automate

[Microsoft Power Automate](https://powerautomate.microsoft.com/) bietet Entwicklern und Entwicklern von Bürgern eine leistungsstarke Möglichkeit, leistungsstarke automatisierte Prozesse zu erstellen, um ihre Unternehmen zu verbessern, ohne Code schreiben zu müssen. Mit dem [Adobe PDF Services](https://us.flow.microsoft.com/en-us/connectors/shared_adobepdftools/adobe-pdf-services/)-Connector als Teil von [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services) können Benutzer beliebige der in der Adobe PDF Services-API in Microsoft Power Automate verfügbaren Aktionen ausführen.

In diesem Tutorial lernen Sie, wie Sie Ihre Zugangsdaten abrufen, um Adobe PDF Services zu verwenden oder zu testen. Abhängig davon, ob Sie ein Benutzer der Testversion oder ein bestehender Kunde sind, führt dieses Tutorial die richtigen Schritte zum Abrufen von Anmeldedaten durch.

## Wie können Benutzer von Microsoft Power Automate den Adobe PDF Services-Connector verwenden?

Vorhandene Microsoft Power Automate-Benutzer können [Testanmeldeinformationen](https://www.adobe.com/go/powerautomate_getstarted_de) für Adobe PDF Services erhalten. Der obige Link ist ein spezieller Anmelde-Link, der speziell für Benutzer von Microsoft Power Automate bei diesem Vorgang behilflich ist.

![Adobe Developer-Benutzer meldet sich an](assets/credentials_1.png)


>[!IMPORTANT]
> Wenn Sie sich für eine Testversion anmelden, müssen Sie eine Adobe ID und keine Enterprise ID verwenden. Wenn Sie aktuell kein Abonnent der Adobe PDF Services-API sind und versuchen, sich mit Ihrer Enterprise ID anzumelden, wird möglicherweise ein Berechtigungsfehler angezeigt, da Ihr Unternehmen Sie nicht zur Verwendung der Adobe PDF Services-API berechtigt hat. Aus diesem Grund wird empfohlen, eine persönliche Adobe ID zu verwenden, die kostenlos ist.
>

1. Nachdem Sie sich angemeldet haben, werden Sie aufgefordert, einen Namen für Ihre neuen Anmeldeinformationen auszuwählen. Geben Sie Ihren *Anmeldeinformationsnamen* ein.
1. Aktivieren Sie das Kontrollkästchen, um den Entwicklerbedingungen zuzustimmen.
1. Wählen Sie **[!UICONTROL Anmeldeinformationen erstellen]** aus.

   ![Ihre Anmeldeinformationen benennen](assets/credentials_2.png)

Diese Anmeldeinformationen decken fünf verschiedene Werte ab:

* Client-ID (API-Schlüssel)
* Geheimer Clientschlüssel
* Organisations-ID
* Technikkonto-ID
* Base64 (codierter privater Schlüssel)

![Neue Anmeldeinformationen](assets/credentials_3.png)

Eine JSON-Datei, die alle diese Werte enthält, wird ebenfalls automatisch auf Ihr System heruntergeladen. Diese Datei hat den Namen &quot;`pdfservices-api-pa-credentials.json`&quot; und sieht wie folgt aus:

```json
{
 "client_id": "client id value",
 "client_secret": "client secret value",
 "organization_id": "organized id value",
 "account_id": "account id value",
 "base64_encoded_private_key": "base64 version of the private key"
}
```

Speichern Sie diese Datei an einem sicheren Speicherort, da es nicht möglich ist, eine Kopie des privaten Schlüssels erneut abzurufen.

### Hinzufügen einer Verbindung in Microsoft Power Automate

Nachdem Sie Ihre Anmeldeinformationen erhalten haben, können Sie sie in Microsoft Power Automate-Workflows verwenden.

1. Öffnen Sie im Randleistenmenü das Menü **[!UICONTROL Daten]** und wählen Sie **Verbindungen** aus:

   ![Verbindungsmenü auf der Microsoft Power Automate-Website](assets/credentials_4.png)

1. Wählen Sie **+ [!UICONTROL Neue Verbindung]** aus.

1. Auf der nächsten Seite wird eine Liste der möglichen Verbindungstypen angezeigt. Geben Sie in der oberen rechten Ecke &quot;adobe&quot; ein, um die Optionen zu filtern:

   ![Liste der Adobe-Verbindungen](assets/credentials_5.png)

1. Wählen Sie **[!UICONTROL Adobe PDF Services (Vorschau)]** aus.
1. Geben Sie im modalen Fenster alle fünf zuvor generierten Werte ein. Wählen Sie **[!UICONTROL Erstellen]**, wenn Sie fertig sind.

   ![Formularfelder zum Eingeben von Anmeldeinformationen](assets/credentials_6.png)

Sie können jetzt Adobe PDF Services in Microsoft Power Automate verwenden.

### Zugriff auf Anmeldedaten nach der Erstellung

Wenn Sie bereits Anmeldeinformationen erstellt und die heruntergeladenen Anmeldeinformationen verlegt haben, können Sie sie in [Adobe Developer Console](https://developer.adobe.com/console) erneut abrufen.

1. Suchen Sie nach der Anmeldung bei [Adobe Developer Console](https://developer.adobe.com/console) zuerst Ihr Projekt und wählen Sie es aus.
1. Wählen Sie im linken Menü unter *Anmeldeinformationen* die Option **Dienstkonto (JWT)** aus:

   ![Vorhandene Anmeldeinformationen](assets/credentials_7.png)

1. Beachten Sie die fünf hier angegebenen Werte: *Client-ID*, *Client-Geheimnis*, *Technische Konto-ID*, *Technische Konto-E-Mail-Adresse* und *Organisations-ID*.

Leider können Sie den vorherigen privaten Schlüssel nicht herunterladen, aber Sie können die Schaltfläche &quot;Öffentliches/Privates Schlüsselpaar generieren&quot; verwenden, um einen neuen Schlüssel zu erstellen.

## Verwenden bestehender Adobe PDF Services-Anmeldedaten

Wenn Sie auf der [!DNL Adobe Acrobat Services]-Website bereits vorhandene Adobe PDF Services-API-Zugangsberechtigungen generiert haben, können Sie diese mit Microsoft Power Automate verwenden. Wenn Sie während der Anmeldung ein SDK heruntergeladen haben, liegen Ihre bestehenden Anmeldeinformationen in Form einer JSON-Datei vor, die höchstwahrscheinlich `pdfservices-api-credentials.json` heißt. Diese JSON-Datei enthält die fünf Schlüssel, die zum Erstellen Ihrer Verbindungsanmeldeinformationen erforderlich sind. Kopieren Sie jeden Wert aus der JSON-Datei in das entsprechende Verbindungsfeld.

Der Wert für den privaten Schlüssel stammt aus einer zweiten Datei mit dem Namen `private.key`.

Sie können die Werte auch von der Adobe Developer Console abrufen, wie oben beschrieben.

## Wie können [!DNL Adobe Acrobat Services] Benutzer mit Microsoft Power Automate arbeiten?

Um mit der Arbeit mit Power Automate zu beginnen, gehen Sie zunächst zu <https://powerautomate.microsoft.com> und verwenden Sie die Schaltfläche &quot;Kostenlos starten&quot;. Wenn du kein Microsoft-Konto hast, musst du eines erstellen. Nach der Anmeldung wird Ihnen das Power Automate-Dashboard angezeigt.

![PA-Dashboard, erste Ansicht](assets/credentials_8.png)

Wie am Anfang dieses Tutorials beschrieben, erstellen Sie einen neuen Flow, fügen Sie einen Schritt hinzu und suchen Sie nach den Adobe PDF Services. Wählen Sie eine Aktion aus, und Sie werden möglicherweise gewarnt, dass ein Premium-Konto erforderlich ist.

![Warnung für Premium-Konto](assets/credentials_9.png)

Wie der obige Screenshot zeigt, können Sie entweder zu einem Arbeitskonto wechseln oder ein neues Organisationskonto einrichten. Sobald Sie diese haben, können Sie die Aktion Adobe PDF Services hinzufügen.

Weitere Informationen zum Erstellen Ihres ersten Microsoft Power Automate-Workflows mit [!DNL Adobe Acrobat Services] finden Sie unter [Erstellen Ihres ersten Workflows in Microsoft Power Automate](https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/pdfservices/create-workflow-power-automate).

## Weitere Ressourcen

Wenn Sie weitere Hilfe benötigen, finden Sie hier eine Liste zusätzlicher Ressourcen:

* Zuerst die Adobe PDF Services Power Automate-Dokumente: <https://docs.microsoft.com/en-us/connectors/adobepdftools/>. Diese Ressourcen ergänzen das, was Sie hier gelernt haben.
* Brauchst du Beispiele? Sie finden zahlreiche [Power Automate-Vorlagen](https://powerautomate.microsoft.com/en-us/connectors/details/shared_adobepdftools/adobe-pdf-services/), die die PDF-Services zeigen.
* Unser Live-Videoinhalt [Paper Clips](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) enthält auch Videos, die die Verwendung von Power Automate zeigen.
* Der [Adobe Tech Blog](https://medium.com/adobetech/tagged/microsoft-power-automate) enthält viele Artikel zum Arbeiten mit Power Automate.
* Wenden Sie sich abschließend auch an die Kerndokumentation zu [PDF Services](https://developer.adobe.com/document-services/docs/overview/).
