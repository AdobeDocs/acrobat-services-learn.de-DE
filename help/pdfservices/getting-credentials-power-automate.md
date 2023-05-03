---
title: Abrufen von Anmeldedaten für Microsoft Power Automate
description: Hier erfahren Sie, wie Sie Anmeldeinformationen abrufen, um Adobe PDF Services zu verwenden oder zu testen.
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-10382.jpg
kt: 10382
exl-id: 68ec654f-74aa-41b7-9103-44df13402032
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '918'
ht-degree: 2%

---

# Abrufen von Anmeldedaten für Microsoft Power Automate

[Microsoft Power Automate](https://powerautomate.microsoft.com/) bietet Entwicklern und Entwicklern eine leistungsstarke Möglichkeit, leistungsstarke automatisierte Prozesse zu erstellen, um ihre Geschäfte zu verbessern, ohne Code schreiben zu müssen. [Adobe PDF Services](https://us.flow.microsoft.com/en-us/connectors/shared_adobepdftools/adobe-pdf-services/) als Teil der [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services)können Benutzer alle in der Adobe PDF Services-API in Microsoft Power Automate verfügbaren Aktionen ausführen.

In diesem Tutorial lernen Sie, wie Sie Ihre Zugangsdaten abrufen, um Adobe PDF Services zu verwenden oder zu testen. Abhängig davon, ob Sie ein Benutzer der Testversion oder ein bestehender Kunde sind, führt dieses Tutorial die richtigen Schritte zum Abrufen von Anmeldedaten durch.

## Wie können Benutzer von Microsoft Power Automate den Adobe PDF Services-Connector verwenden?

Bestehende Benutzer von Microsoft Power Automate können [Testanmeldeinformationen abrufen](https://www.adobe.com/go/powerautomate_getstarted_de) für Adobe PDF Services. Der obige Link ist ein spezieller Anmelde-Link, der speziell für Benutzer von Microsoft Power Automate bei diesem Vorgang behilflich ist.

![Adobe Developer-Benutzeranmeldung](assets/credentials_1.png)


>[!IMPORTANT]
> Wenn Sie sich für eine Testversion anmelden, müssen Sie eine Adobe ID und keine Enterprise ID verwenden. Wenn Sie aktuell kein Abonnent der Adobe PDF Services-API sind und versuchen, sich mit Ihrer Enterprise ID anzumelden, wird möglicherweise ein Berechtigungsfehler angezeigt, da Ihr Unternehmen Sie nicht zur Verwendung der Adobe PDF Services-API berechtigt hat. Aus diesem Grund wird empfohlen, eine persönliche Adobe ID zu verwenden, die kostenlos ist.

1. Nachdem Sie sich angemeldet haben, werden Sie aufgefordert, einen Namen für Ihre neuen Anmeldeinformationen auszuwählen. Geben Sie Ihr *Berechtigungsname*.
1. Aktivieren Sie das Kontrollkästchen, um den Entwicklerbedingungen zuzustimmen.
1. Auswählen **[!UICONTROL Anmeldeinformationen erstellen]**.

   ![Ihre Anmeldeinformationen benennen](assets/credentials_2.png)

Diese Anmeldeinformationen decken fünf verschiedene Werte ab:

* Client-ID (API-Schlüssel)
* Geheimer Clientschlüssel
* Organisations-ID
* Technikkonto-ID
* Base64 (codierter privater Schlüssel)

![Neue Anmeldedaten](assets/credentials_3.png)

Eine JSON-Datei, die alle diese Werte enthält, wird ebenfalls automatisch auf Ihr System heruntergeladen. Diese Datei hat den Namen `pdfservices-api-pa-credentials.json` und sieht wie folgt aus:

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

1. Öffnen Sie im Randleistenmenü die **[!UICONTROL Daten]** und wählen Sie **Verbindungen**:

   ![Verbindungsmenü auf der Microsoft Power Automate-Website](assets/credentials_4.png)

1. Auswählen **+ [!UICONTROL Neue Verbindung]**.

1. Auf der nächsten Seite wird eine Liste der möglichen Verbindungstypen angezeigt. Geben Sie in der oberen rechten Ecke &quot;adobe&quot; ein, um die Optionen zu filtern:

   ![Liste der Adoben](assets/credentials_5.png)

1. Auswählen **[!UICONTROL Adobe PDF Services (Vorschau)]**.
1. Geben Sie im modalen Fenster alle fünf zuvor generierten Werte ein. Auswählen **[!UICONTROL Erstellen]** wenn Sie fertig sind.

   ![Formularfelder zum Eingeben von Anmeldeinformationen](assets/credentials_6.png)

Sie können jetzt Adobe PDF Services in Microsoft Power Automate verwenden.

### Zugriff auf Anmeldedaten nach der Erstellung

Wenn Sie bereits Anmeldeinformationen erstellt und die heruntergeladenen Anmeldeinformationen falsch platziert haben, können Sie sie erneut abrufen in [Adobe Developer Console](https://developer.adobe.com/console).

1. Nach der Anmeldung bei [Adobe Developer Console](https://developer.adobe.com/console), suchen Sie zunächst Ihr Projekt und wählen Sie es aus.
1. Im linken Menü unter *Anmeldedaten*&quot; die Option **Dienstkonto (JWT)**:

   ![Bestehende Anmeldedaten](assets/credentials_7.png)

1. Beachten Sie die fünf hier angezeigten Werte: *Client-ID*, *Client-Geheimnis*, *Technische Konto-ID*, *Technische Konto-E-Mail* und *Organisations-ID*.

Leider können Sie den vorherigen privaten Schlüssel nicht herunterladen, aber Sie können die Schaltfläche &quot;Öffentliches/Privates Schlüsselpaar generieren&quot; verwenden, um einen neuen Schlüssel zu erstellen.

## Verwenden bestehender Adobe PDF Services-Anmeldedaten

Wenn Sie bereits über Adobe PDF Services-API-Zugangsberechtigungen verfügen, die von [!DNL Adobe Acrobat Services] Website verwenden, können Sie sie mit Microsoft Power Automate verwenden. Wenn Sie während der Anmeldung ein SDK heruntergeladen haben, liegen Ihre bestehenden Anmeldeinformationen in Form einer JSON-Datei vor, die höchstwahrscheinlich den Namen `pdfservices-api-credentials.json`. Diese JSON-Datei enthält die fünf Schlüssel, die zum Erstellen Ihrer Verbindungsinformationen erforderlich sind. Kopieren Sie jeden Wert aus der JSON-Datei in das entsprechende Verbindungsfeld.

Der Wert für den privaten Schlüssel stammt aus einer zweiten Datei mit dem Namen `private.key`.

Sie können die Werte auch wie oben beschrieben aus der Adobe Developer-Konsole abrufen.

## Wie kann ich [!DNL Adobe Acrobat Services] Benutzer beginnen mit der Arbeit mit Microsoft Power Automate?

Um mit Power Automate zu arbeiten, gehen Sie zunächst zu <https://powerautomate.microsoft.com> klicken Sie auf &quot;Kostenlos starten&quot;. Wenn du kein Microsoft-Konto hast, musst du eines erstellen. Nach der Anmeldung wird Ihnen das Power Automate-Dashboard angezeigt.

![PA-Dashboard, erste Ansicht](assets/credentials_8.png)

Wie am Anfang dieses Tutorials beschrieben, erstellen Sie einen neuen Flow, fügen Sie einen Schritt hinzu und suchen Sie nach den Adobe PDF Services. Wählen Sie eine Aktion aus, und Sie werden möglicherweise gewarnt, dass ein Premium-Konto erforderlich ist.

![Warnung für Premium-Konto](assets/credentials_9.png)

Wie der obige Screenshot zeigt, können Sie entweder zu einem Arbeitskonto wechseln oder ein neues Organisationskonto einrichten. Sobald Sie diese haben, können Sie die Aktion Adobe PDF Services hinzufügen.

Einen genaueren Blick auf die Erstellung Ihres ersten Microsoft Power Automate-Workflows mit [!DNL Adobe Acrobat Services], siehe [Erstellen Sie Ihren ersten Arbeitsablauf in Microsoft Power Automate](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfservices/create-workflow-power-automate.html).

## Weitere Ressourcen

Wenn Sie weitere Hilfe benötigen, finden Sie hier eine Liste zusätzlicher Ressourcen:

* An erster Stelle stehen die Adobe PDF Services Power Automate-Dokumente: <https://docs.microsoft.com/en-us/connectors/adobepdftools/>. Diese Ressourcen ergänzen das, was Sie hier gelernt haben.
* Brauchst du Beispiele? Hier finden Sie zahlreiche [Power Automate-Vorlagen](https://powerautomate.microsoft.com/en-us/connectors/details/shared_adobepdftools/adobe-pdf-services/) die PDF-Services.
* Unsere Live-Videoinhalte, [Papierklammern](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF)enthält auch Videos, die die Verwendung von Power Automate zeigen.
* Die [Adobe Tech Blog.](https://medium.com/adobetech/tagged/microsoft-power-automate) hat viele Artikel zum Arbeiten mit Power Automate.
* Wenden Sie sich abschließend an den [PDF Services](https://developer.adobe.com/document-services/docs/overview/) -Dokumentation.
