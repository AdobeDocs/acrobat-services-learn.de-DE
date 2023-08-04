---
title: Erstellen Sie Ihren ersten Arbeitsablauf in Microsoft Power Automate
description: Erfahren Sie, wie Sie den Adobe PDF Services-Connector in Microsoft Power Automate verwenden.
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-10379
thumbnail: KT-10379.jpg
exl-id: 095b705f-c380-42cc-9329-44ef7de655ee
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '1999'
ht-degree: 2%

---


# Erstellen Sie Ihren ersten Flow in Microsoft Power Automate

Erfahren Sie, wie Sie Ihren ersten Flow in [Microsoft Power Automate](https://flow.microsoft.com) mit dem Katalog [Adobe PDF Services](https://us.flow.microsoft.com/en-us/connectors/shared_adobepdftools/adobe-pdf-services/) Connector.

In diesem praktischen Tutorial lernen Sie Folgendes:

* Word-Dokumente in PDF konvertieren
* Zusammenführen von PDF-Dokumenten zu einer PDF
* Protect eines PDF-Dokuments mit einem Kennwort

## Vorbereitung

### Was Sie benötigen

* **Probe-Abo oder Produktions-Anmeldedaten für Adobe PDF Services**
Weitere Informationen zum Abrufen und Konfigurieren von Anmeldeinformationen in Microsoft Power Automate [hier](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfservices/getting-credentials-power-automate.html).
* **Microsoft Power Automate mit Premium-Connectors**
Erfahren Sie, wie Sie den Lizenzierungsgrad für Power Automate überprüfen. [hier](https://docs.microsoft.com/en-us/power-platform/admin/power-automate-licensing/types).
* **OneDrive**
Der OneDrive-Speicheranschluss wird in diesem Tutorial verwendet, aber jeder Speicheranschluss kann ersetzt werden.

### Beispieldateien

Es gibt zwei [Beispieldateien](assets/sample-assets.zip) die Sie entpacken und auf OneDrive hochladen müssen:

* WordDocument01.docx
* WordDocument02.docx

### Abrufen von Anmeldedaten

Zum Durchführen dieses Tutorials benötigen Sie Ihre in Microsoft Power Automate für Adobe PDF Services bereits konfigurierten Anmeldeinformationen. Wenn Sie diesen Schritt nicht abgeschlossen haben, finden Sie weitere Informationen unter [Hier finden Sie Anweisungen](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfservices/getting-credentials-power-automate.html).

## Teil 1: Erstellen eines neuen Textflusses und Konvertieren von Word in PDF

### Textfluss erstellen

In diesem Teil erstellen Sie einen neuen Textfluss in [Microsoft Power Automate](https://flow.microsoft.com) Verwenden Sie einen sofortigen Flow, fügen Sie Parameter hinzu, rufen Sie Ihre Dateien aus OneDrive ab und konvertieren Sie sie in PDF.

1. Navigieren Sie zu [Microsoft Power Automate](https://flow.microsoft.com) und melden Sie sich mit Ihren Anmeldedaten an.
1. Wählen Sie in der Seitenleiste **[!UICONTROL Erstellen]**.

   ![Schaltfläche Erstellen](assets/createButtonPowerAutomate.png)

1. Auswählen **[!UICONTROL Sofortfluss]**.
1. Geben Sie Ihrem Flow einen Namen.
1. Unter *Auswählen, wie dieser Fluss ausgelöst werden soll*&quot; die Option **[!UICONTROL Manuelles Auslösen von Textflüssen]**.
1. Wählen Sie **[!UICONTROL Erstellen]** aus.

### Dateiinhalte von Dateien abrufen

Rufen Sie als Nächstes den Dateiinhalt der Beispieldateien ab.

>[!PREREQUISITES]
>
>Wenn Sie die Datei [Beispieldateien](assets/sample-assets.zip) Entpacken Sie sie und laden Sie sie hoch.


1. In [Power Automate](https://flow.microsoft.com)&quot; die Option **[!UICONTROL + Neuer Schritt]**.
1. Suchen nach *OneDrive* in der Suchleiste.
1. Wählen Sie entweder Ihr geschäftliches oder persönliches OneDrive-Konto aus, indem Sie auf **[!UICONTROL OneDrive for Business]** oder **[!UICONTROL OneDrive]**.
1. Suchen nach *Dateiinhalt abrufen* in der Suchleiste.
1. Im Dialogfeld &quot; **[!UICONTROL Datei]** auf das Ordnersymbol , um zum Dialogfeld &quot; *WordDocument01.docx* in OneDrive gespeichert.

   ![Abrufen von Dateiinhalten mit der OneDrive-Aktion in Microsoft Power Automate](assets/getFileContentOneDrive.png)

### Datei auf PDF konvertieren

Nachdem Sie den Dateiinhalt fertig haben, können Sie das Dokument zum PDF konvertieren.

1. In [Power Automate](https://flow.microsoft.com)&quot; die Option **[!UICONTROL + Neuer Schritt]**.
1. Suchen nach *Adobe PDF Services* in der Suchleiste.
1. Auswählen **[!UICONTROL Adobe PDF Services]**.
1. Suchen nach *Word-Datei nach PDF konvertieren* in der Suchleiste.
1. In **[!UICONTROL Dateiname]**, benennen Sie Ihre Datei wie gewünscht, aber sie muss mit *.docx*. Diese Erweiterung ist für die Konvertierung von Dokumenten von Word in PDF erforderlich.
1. Platzieren Sie den Cursor im **[!UICONTROL Dateiinhalt]** ein.
1. Im Fenster &quot; **[!UICONTROL Dynamischer Inhalt]** &quot; die Option **[!UICONTROL Dateiinhalt]**.

   ![Konvertieren von Word in PDF in Microsoft Power Automate](assets/convertWordToPDFActionPowerAutomate.png)

### Speichern Sie die Datei auf OneDrive

Wenn das Dokument generiert wurde, speichern Sie die Datei wieder in OneDrive.

1. In [Microsoft Power Automate](https://flow.microsoft.com)&quot; die Option **[!UICONTROL + Neuer Schritt]**.
1. Suchen nach *OneDrive* in der Suchleiste.
1. Wählen Sie entweder Ihr geschäftliches oder persönliches OneDrive-Konto aus, indem Sie auf **[!UICONTROL OneDrive for Business]** oder **[!UICONTROL OneDrive]**.
1. Suchen nach *Dateiinhalt abrufen* in der Suchleiste.
1. Suchen nach *Datei erstellen* in der Suchleiste.
1. Auswählen **[!UICONTROL Datei erstellen]**.
1. Im Dialogfeld &quot; **[!UICONTROL Ordnerpfad]** &quot; das Ordnersymbol aus, um anzugeben, wo die Datei in OneDrive gespeichert werden soll.
1. In **[!UICONTROL Dateiname]**, benennen Sie Ihre Datei wie gewünscht, aber sie muss mit *.docx*. Diese Erweiterung ist für die Konvertierung von Dokumenten von Word in PDF erforderlich.
1. Im Dialogfeld &quot; **[!UICONTROL Dateiinhalt]** ein, verwenden Sie **[!UICONTROL Dynamischer Inhalt]** &quot;, um die Variable &quot;PDF-Dateiinhalt&quot; einzufügen.

### Flow testen

1. Wählen Sie oben links **[!UICONTROL Untitled]** , um den Textfluss umzubenennen.
1. Wählen Sie **[!UICONTROL Speichern]** aus.
1. Auswählen **[!UICONTROL Test]**.
1. Auswählen **[!UICONTROL Manuell]** und dann **[!UICONTROL Speichern und testen]**.
1. Klicken Sie auf **[!UICONTROL Weiter]**.
1. Auswählen **[!UICONTROL Textfluss ausführen]**.

Im OneDrive-Ordner sollte jetzt die konvertierte PDF angezeigt werden.

![Ausgewähltes konvertiertes PDF-Dokument in OneDrive](assets/selectedGeneratedFileInOneDrive.png)

## Teil 2: Dynamisches Dokument aus Vorlage generieren

Dieser nächste Teil baut auf Teil 1 auf und verwendet die *Generieren von Dokumenten aus Word* , um Daten dynamisch in Ihrem Dokument zusammenzuführen.

### Dokumentvorlage überprüfen

Öffnen *WordDocument02_.docx* aus den Beispieldateien in OneDrive. Das Word-Dokument enthält verschiedene Text-Tags, die Stellen darstellen, an denen Daten in das Dokument eingefügt werden.

### Parameter für Auslöser hinzufügen

Damit dynamische Daten in das Dokument übertragen werden, müssen Sie einige Parameter für den Trigger erstellen, um Werte anzugeben.

1. Wenn Sie Ihren Flow bearbeiten, wählen Sie **[!UICONTROL Manuelles Auslösen von Textflüssen]** , um die Aktion zu erweitern.
1. Auswählen **[!UICONTROL Eingabe hinzufügen]**.
1. Auswählen **[!UICONTROL Text]**.
1. Feld benennen *Vorname*.

Wiederholen Sie die Schritte 2-4, um die folgenden Felder hinzuzufügen:

* Nachname
* Gehalt

![Trigger in Power Automate mit Parameterfeldern](assets/triggerParametersInPowerAutomate.png)

### Dateiinhalt einer Vorlage abrufen

Um ein Dokument zu generieren, müssen Sie zunächst den Dateiinhalt der Word-Vorlage abrufen.

1. Wählen Sie in Power Automate + aus. **[!UICONTROL Neuer Schritt]**.
1. Suchen nach *OneDrive* in der Suchleiste.
1. Wählen Sie entweder Ihr geschäftliches oder persönliches OneDrive-Konto aus, indem Sie auf **[!UICONTROL OneDrive for Business]** oder **[!UICONTROL OneDrive]**.
1. Suchen nach *Dateiinhalt abrufen* in der Suchleiste.
1. Im Dialogfeld &quot; **[!UICONTROL Datei]** auf das Ordnersymbol , um zum Dialogfeld &quot; *WordDocument02.docx* in OneDrive gespeichert.

![Abrufen von Dateiinhalten aus OneDrive in Microsoft Power Automate](assets/getFileContentAction02.png)

### Dokument aus Vorlage generieren

1. Wählen Sie in Power Automate **[!UICONTROL + Neuer Schritt]**.
1. Suchen nach *Adobe PDF Services* in der Suchleiste.
1. Auswählen **[!UICONTROL Adobe PDF Services]**.
1. Wählen Sie das **[!UICONTROL Dokument aus Word-Vorlage generieren]** action angezeigt.
1. Im Dialogfeld &quot; **[!UICONTROL Vorlagendateiname]** -Feld, benennen Sie Ihre Datei wie gewünscht, aber es muss mit *.docx*.

#### Daten zusammenführen

Im Fenster &quot; *Dokument aus Word-Vorlage generieren* -Aktion können Sie mithilfe von dynamischem Inhalt Daten aus einer beliebigen der verschiedenen Variablen, die sich zuvor im Textfluss befanden, in Ihr Dokument zusammenführen.

Kopieren Sie die folgenden JSON-Daten in die Datei **Daten zusammenführen** Feld:

```
{
    "FirstName": "",
    "LastName": "",
    "Salary": ""
}
```

1. Platzieren Sie den Cursor in das Feld zwischen den beiden Anführungszeichen für die *FirstName* Wert.
1. Im Fenster &quot; **[!UICONTROL Dynamischer Inhalt]** &quot; das Element *Vorname* aus der Aktion Flow manuell auslösen.

   ![Generieren von Dokumenten mit Daten-Tags in JSON](assets/generateDocumentJSONAction.png)

1. Wiederholen Sie die Schritte 7-8 für die **[!UICONTROL LastName]** und **[!UICONTROL Gehalt]** Felder.
1. Im Dialogfeld &quot; **[!UICONTROL Vorlagendateiinhalt]** das Feld **[!UICONTROL Dynamischer Inhalt]** , um das **[!UICONTROL Dateiinhalt]** Wert aus der *Dateiinhalt abrufen* -Schritt.

![Generieren eines Dokuments aus einer Word-Vorlagenaktion in Power Automate mit abgeschlossenen Werten](assets/generateDocumentJSONActionCompleted.png)

>[!TIP]
>
>Die *Dokument aus Word-Vorlage generieren* verwendet die Adobe Document Generation API. Wenn Sie mehr über das Erstellen von Vorlagen erfahren möchten, finden Sie hier einige Ressourcen:
>
>* [Weitere Informationen zur Generierung von Adobe-Dokumenten](https://developer.adobe.com/document-services/apis/doc-generation/)
>* [Adobe-Tagger für die Dokumentgenerierung für Microsoft Word](https://appsource.microsoft.com/en-US/product/office/WA200002654)
>* [Adobe Dokumentenerzeugung API - Dokumentation](https://developer.adobe.com/document-services/docs/overview/document-generation-api/)

### Speichern Sie die Datei auf OneDrive

Sobald das Dokument generiert wurde, können Sie die Datei wieder in OneDrive speichern.

1. Wählen Sie in Power Automate **+ [!UICONTROL Neuer Schritt]**.
1. Suchen nach *OneDrive* in der Suchleiste.
1. Wählen Sie entweder Ihr geschäftliches oder persönliches OneDrive-Konto aus, indem Sie auf **[!UICONTROL OneDrive for Business]** oder **[!UICONTROL OneDrive]**.
1. Suchen nach *Datei erstellen* in der Suchleiste.
1. Auswählen **[!UICONTROL Datei erstellen]**.
1. Im Dialogfeld &quot; **[!UICONTROL Ordnerpfad]** &quot; das Ordnersymbol aus, um anzugeben, wo die Datei in OneDrive gespeichert werden soll.
1. Im Dialogfeld &quot; **[!UICONTROL Dateiname]** den Namen der Datei fest. Da es sich bei der Ausgabe um eine PDF handelt, muss der Dateiname mit der Erweiterung .pdf enden.
1. Verwenden Sie die **[!UICONTROL Dynamischer Inhalt]** &quot;, um die Variable &quot;PDF-Dateiinhalt&quot; in das Dialogfeld **[!UICONTROL Dateiinhalt]** ein.

### Flow testen

![Flow-Bildschirm in Microsoft Power Automate ausführen, um zur Eingabe aufzufordern](assets/runFlowParameters.png)

1. Wählen Sie **[!UICONTROL Speichern]** aus.
1. Auswählen **[!UICONTROL Test]**.
1. Auswählen **[!UICONTROL Manuell]** und dann **[!UICONTROL Speichern und testen]**.
1. Klicken Sie auf **[!UICONTROL Weiter]**.
1. Geben Sie Werte für *Vorname*, *Nachname* und *Gehalt*.
1. Auswählen **[!UICONTROL Textfluss ausführen]**.

Im OneDrive-Ordner wird jetzt eine aus dem Word-Dokument generierte PDF angezeigt. Wenn Sie das PDF-Dokument in OneDrive öffnen, sehen Sie, dass die Daten in den Text-Tag-Speicherorten zusammengeführt werden.


## Teil 3: PDF in einem Programm kombinieren

Nachdem Sie nun ein Word-Dokument erstellt und in eine PDF konvertiert haben, können Sie als Nächstes mehrere PDF-Dokumente kombinieren.

>[!NOTE]
>
>In den vorherigen Aktionen haben Sie eine Kopie des Dokuments als Datei in OneDrive gespeichert. Um Tools wie Merge-PDF verwenden zu können, müssen Sie die Datei nicht auf OneDrive speichern. Stattdessen können Sie die Ausgabe direkt von einer Aktion an die nächste übergeben, was besser ist, als nach jeder Aktion in OneDrive zu speichern. Zu Demonstrationszwecken speichern Sie diese Dateien jedoch auf OneDrive.

### Schritt &quot;Zusammenführen-PDF hinzufügen&quot;

1. Wenn Sie Ihren Flow bearbeiten, wählen Sie **[!UICONTROL + Nächster Schritt]** , um am Ende Ihres Flows eine Aktion hinzuzufügen.
1. Suchen nach *Adobe PDF Services* in der Suchleiste.
1. Auswählen **[!UICONTROL Adobe PDF Services]**.
1. Wählen Sie das **[!UICONTROL PDF zusammenführen]** Aktion.
1. Im Dialogfeld &quot; **[!UICONTROL Name der PDF-Zusammenführungsdatei]** geben Sie Ihren gewünschten Dateinamen ein (z. B.*CombinedDocument.pdf*).
1. Im Dialogfeld &quot; **[!UICONTROL Dateiinhalt -1]** das Feld **[!UICONTROL Dynamischer Inhalt]** , um das *PDF von Dateiinhalten* Wert aus der **[!UICONTROL Word-Datei nach PDF konvertieren]** -Schritt.
1. Um das nächste Dokument hinzuzufügen, wählen Sie **+ [!UICONTROL Neues Element hinzufügen]**.
1. Im Dialogfeld &quot; **[!UICONTROL Dateiinhalt - 2]** das Feld **[!UICONTROL Dynamischer Inhalt]** , um das **[!UICONTROL Inhalt der Ausgabedatei]** Wert aus der *Dokument aus Word-Vorlage generieren* -Schritt.

![PDF-Zusammenführungsaktion in Microsoft Power Automate](assets/mergePDFAction.png)

### Speichern einer zusammengeführten PDF auf OneDrive

Nachdem das Dokument kombiniert wurde, können Sie es wieder in OneDrive speichern.

1. Wählen Sie in Power Automate **+ [!UICONTROL Neuer Schritt]**.
1. Suchen nach *OneDrive* in der Suchleiste.
1. Wählen Sie entweder Ihr geschäftliches oder persönliches OneDrive-Konto aus, indem Sie auf **[!UICONTROL OneDrive for Business]** oder **[!UICONTROL OneDrive]**.
1. Suchen nach *Datei erstellen* in der Suchleiste.
1. Auswählen **[!UICONTROL Datei erstellen]**.
1. Im Dialogfeld &quot; **[!UICONTROL Ordnerpfad]** &quot; das Ordnersymbol aus, um anzugeben, wo die Datei in OneDrive gespeichert werden soll.
1. Im Dialogfeld &quot; **[!UICONTROL Dateiname]** den Namen der Datei fest. Da es sich bei der Ausgabe um eine PDF handelt, muss der Dateiname mit .pdf enden.
1. Im Dialogfeld &quot; **[!UICONTROL Dateiinhalt]** ein, verwenden Sie **[!UICONTROL Dynamischer Inhalt]** , um das *PDF von Dateiinhalten* Wert aus der **[!UICONTROL PDF zusammenführen]** -Schritt.

   ![Flow in Microsoft Power Automate - Übersicht](assets/flowOverviewSavedMergedDocument.png)

### Flow testen

1. Wählen Sie **[!UICONTROL Speichern]** aus.
1. Auswählen **[!UICONTROL Test]**.
1. Auswählen **[!UICONTROL Manuell]** und dann **[!UICONTROL Speichern und testen]**.
1. Klicken Sie auf **[!UICONTROL Weiter]**.
1. Geben Sie Werte für *Vorname*, *Nachname* und *Gehalt*.
1. Auswählen **[!UICONTROL Textfluss ausführen]**.

Im OneDrive-Ordner wird die zusammengeführte PDF mit den Seiten aus dem ersten und zweiten Dokument angezeigt.

## Teil 4: Protect PDF-Dokument

Nach dem Generieren Ihres Dokuments können Sie es vor der Bearbeitung schützen, indem Sie vor dem Speichern in OneDrive einen zusätzlichen Schritt hinzufügen.

### PDF schützen

1. Wählen Sie beim Bearbeiten Ihres Workflows in Power Automate **+** zwischen den **[!UICONTROL PDF zusammenführen]** und die **[!UICONTROL Datei 3 erstellen]** Aktion.

   ![Pluszeichen zwischen zwei Aktionen zum Hinzufügen einer neuen Aktion](assets/addActionToProtect.png)

1. Auswählen **[!UICONTROL Aktion hinzufügen]**.
1. Suchen nach *Adobe PDF Services* in der Suchleiste.
1. Auswählen **[!UICONTROL Adobe PDF Services]**.
1. Wählen Sie das **[!UICONTROL Protect PDF von der Anzeige]** Aktion.
1. Im Dialogfeld &quot; **[!UICONTROL Dateiname]** &quot; den Namen auf den gewünschten Namen fest, sofern er mit der Erweiterung .pdf endet.
1. Legen Sie die **[!UICONTROL Kennwort]** auf Ihr angegebenes Kennwort, um das Dokument zu öffnen.
1. Im Dialogfeld &quot; **[!UICONTROL Dateiinhalt]** das Feld **[!UICONTROL Dynamischer Inhalt]** , um das *PDF von Dateiinhalten* Wert aus der **[!UICONTROL PDF zusammenführen]** -Schritt.

### Speichern auf OneDrive aktualisieren

Sobald das Dokument geschützt ist, können Sie die Datei wieder in OneDrive speichern. In diesem Beispiel aktualisieren Sie die bereits vorhandene **Datei 3 erstellen** Aktion mit einer neuen *Dateiinhalt* Wert.

1. Wählen Sie den Cursor in der **[!UICONTROL Dateiinhalt]** im Feld **[!UICONTROL Datei 3 erstellen]** Aktion.
1. Verwenden Sie die **[!UICONTROL Dynamischer Inhalt]** , um das *PDF von Dateiinhalten* Wert aus der **Protect PDF von der Anzeige** -Schritt.

### Flow testen

1. Wählen Sie **[!UICONTROL Speichern]** aus.
1. Auswählen **[!UICONTROL Test]**.
1. Auswählen **[!UICONTROL Manuell]** und dann **[!UICONTROL Speichern und testen]**.
1. Klicken Sie auf **[!UICONTROL Weiter]**.
1. Geben Sie Werte für *Vorname*, *Nachname* und *Gehalt*.
1. Auswählen **[!UICONTROL Textfluss ausführen]**.

Im OneDrive-Ordner wird die kombinierte PDF angezeigt, auf der Sie jetzt aufgefordert werden, ein Kennwort einzugeben, um das Dokument anzuzeigen.

## Nächste Schritte

In diesem Tutorial haben Sie ein Word-Dokument in einen PDF konvertiert, ein Dokument auf der Grundlage von Daten generiert, Dokumente zusammengeführt und mit einem Kennwort geschützt. Um mehr zu erfahren, erkunden Sie einige der anderen Aktionen, die im Adobe PDF Services-Connector in Microsoft Power Automate verfügbar sind:

* Sehen Sie sich die in Microsoft Power Automate verfügbaren vorgefertigten Vorlagen an.
* Von Adobe lernen [Artikel](https://medium.com/adobetech/tagged/microsoft-power-automate) im Adobe Tech Blog.
* Review [Dokumentation](https://developer.adobe.com/document-services/docs/overview/document-generation-api/) für die Adobe Document Generation API.
