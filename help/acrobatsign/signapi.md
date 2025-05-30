---
title: Erste Schritte mit der Acrobat Sign API
description: Erfahren Sie, wie Sie die Acrobat Sign API in Ihre Anwendung einbinden, um Signaturen und andere Informationen zu erfassen
feature: Acrobat Sign API
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8089
thumbnail: KT-8089.jpg
exl-id: ae1cd9db-9f00-4129-a2a1-ceff1c899a83
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1906'
ht-degree: 0%

---

# Erste Schritte mit der Adobe Sign API

Die [Acrobat Sign-API](https://developer.adobe.com/adobesign-api/) ist eine großartige Möglichkeit, die Verwaltung signierter Vereinbarungen zu verbessern. Entwickler können ihre Systeme ganz einfach mit der Sign-API integrieren, die eine zuverlässige, einfache Möglichkeit bietet, Dokumente hochzuladen, zum Signieren zu senden, Erinnerungen zu senden und elektronische Signaturen zu erfassen.

## Lernziel.

In diesem praktischen Tutorial wird erläutert, wie Entwickler die Sign-API verwenden können, um mit [!DNL Adobe Acrobat Services] erstellte Anwendungen und Workflows zu verbessern. [!DNL Acrobat Services] umfasst [Adobe PDF Services API](https://developer.adobe.com/document-services/apis/pdf-services), [Adobe PDF Embed API](https://developer.adobe.com/document-services/apis/pdf-embed/) (kostenlos) und [Adobe Document Generation API](https://developer.adobe.com/document-services/apis/doc-generation).

Insbesondere erfahren Sie, wie Sie die Acrobat Sign API in Ihre Anwendung integrieren, um Signaturen und andere Informationen, wie z. B. Mitarbeiterinformationen in einem Versicherungsformular, zu erfassen. Es werden generische Schritte mit vereinfachten HTTP-Anforderungen und -Antworten verwendet. Sie können diese Anforderungen in Ihrer bevorzugten Sprache implementieren. Sie können eine PDF mit einer Kombination aus [[!DNL Acrobat Services] APIs](https://developer.adobe.com/document-services/homepage/) erstellen, sie als [temporäres](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/overview/terminology.md) Dokument in die Sign-API hochladen und mithilfe der Vereinbarung oder des [Widget](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/overview/terminology.md)-Workflows Endbenutzersignaturen anfordern.

## Erstellen eines PDF-Dokuments

Lege eine Microsoft Word-Vorlage an, und speichere sie auf dem PDF. Oder Sie können Ihre Pipeline mithilfe der Dokumentenerzeugung-API automatisieren, um eine in Word erstellte Vorlage hochzuladen und dann ein PDF-Dokument zu generieren. Die Dokumentengenerierungs-API ist Teil von [!DNL Acrobat Services], [, sechs Monate lang kostenlos, dann &quot;Pay-as-you-go&quot; für nur 0,05 $ pro Dokumenttransaktion](https://developer.adobe.com/document-services/pricing/main).

In diesem Beispiel ist die Vorlage nur ein einfaches Dokument mit einigen Unterzeichnerfeldern, die ausgefüllt werden müssen. Nennen Sie die Felder vorerst und fügen Sie später die tatsächlichen Felder in dieses Tutorial ein.

![Screenshot des Versicherungsformulars mit einigen Feldern](assets/GSASAPI_1.png)

## Ermitteln des gültigen API-Zugriffspunkts

Erstellen Sie vor der Arbeit mit der Sign-API [ein kostenloses Entwicklerkonto](https://acrobat.adobe.com/ca/en/sign/developer-form.html), um auf die API zuzugreifen, den Austausch und die Ausführung von Dokumenten zu testen und die E-Mail-Funktion zu testen.

Adobe verteilt die Acrobat Sign-API in vielen Bereitstellungseinheiten, so genannten &quot;Shards&quot;, rund um den Globus. Jeder Shard dient dem Konto eines Kunden, z. B. NA1, NA2, NA3, EU1, JP1, AU1, IN1 und andere. Die Shard-Namen entsprechen geografischen Standorten. Diese Shards bilden den Basis-URI (Zugriffspunkte) der API-Endpunkte.

Um auf die Sign-API zuzugreifen, müssen Sie zunächst den richtigen Zugriffspunkt für Ihr Konto ermitteln, der je nach Speicherort api.na1.adobesign.com, api.na4.adobesign.com, api.eu1.adobesign.com oder andere sein kann.

```
  GET /api/rest/v6/baseUris HTTP/1.1
  Host: https://api.adobesign.com
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  Accept: application/json

  Response Body (example):

  {
    "apiAccessPoint": "https://api.na4.adobesign.com/", 
    "webAccessPoint": "https://secure.na4.adobesign.com/" 
  }
```

Im obigen Beispiel ist eine Antwort mit dem Wert als Zugriffspunkt.

>[!IMPORTANT]
>
>In diesem Fall müssen alle nachfolgenden Anforderungen, die Sie an die Sign-API stellen, diesen Zugriffspunkt verwenden. Wenn Sie einen Zugriffspunkt verwenden, der nicht Ihrer Region dient, wird eine Fehlermeldung angezeigt.

## Hochladen eines temporären Dokuments

Mit Adobe Sign können Sie verschiedene Flows erstellen, die Dokumente für Signaturen oder Datenerfassung vorbereiten. Unabhängig vom Ablauf Ihrer Anwendung müssen Sie zuerst ein Dokument hochladen, das nur sieben Tage lang verfügbar ist. Die nachfolgenden API-Aufrufe müssen dann auf dieses temporäre Dokument verweisen.

Das Dokument wird mithilfe einer POST-Anforderung an den Endpunkt `/transientDocuments` hochgeladen. Die mehrteilige Anforderung besteht aus dem Dateinamen, einem Dateistream und dem MIME-Typ (Medien) der Dokumentdatei. Die Endpunktantwort enthält eine ID, die das Dokument identifiziert.

Darüber hinaus kann Ihre Anwendung eine Rückruf-URL für Acrobat Sign zum Pingen angeben, wobei die App benachrichtigt wird, wenn der Signaturvorgang abgeschlossen ist.


```
  POST /api/rest/v6/transientDocuments HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  x-api-user: email:your-api-user@your-domain.com
  Content-Type: multipart/form-data
  File-Name: "Insurance Form.pdf"
  File: "[path]\Insurance Form.pdf"
  Accept: application/json

  Response Body (example):

  {
     "transientDocumentId": "3AAA...BRZuM"
  }
```

## Erstellen eines Webformulars

Webformulare (früher als Signatur-Widgets bezeichnet) sind gehostete Dokumente, die jeder signieren kann, der über Zugriff verfügt. Beispiele für Webformulare sind Anmeldungsblätter, Haftungsausschlüsse und andere Dokumente, auf die viele Personen zugreifen und die sie online signieren.

Um ein neues Webformular mit der Sign-API zu erstellen, müssen Sie zunächst ein temporäres Dokument hochladen. Die POST-Anforderung an den Endpunkt &quot;`/widgets`&quot; verwendet die zurückgegebene `transientDocumentId` .

In diesem Beispiel ist das Webformular `ACTIVE`, Sie können es jedoch in einem von drei verschiedenen Status erstellen:

* ENTWURF - zum schrittweisen Erstellen des Webformulars

* AUTHORING - zum Hinzufügen oder Bearbeiten von Formularfeldern im Webformular

* ACTIVE: Sofortiges Hosten des Webformulars

Die Angaben zu den Teilnehmern des Formulars müssen ebenfalls festgelegt werden. Die `memberInfos`-Eigenschaft enthält Daten zu den Teilnehmern, z. B. E-Mail. Derzeit unterstützt diese Gruppe nicht mehr als ein Mitglied. Da die E-Mail-Adresse des Webformular-Unterzeichners zum Zeitpunkt der Erstellung des Webformulars jedoch unbekannt ist, sollte die E-Mail-Adresse leer gelassen werden, wie im folgenden Beispiel dargestellt. Die `role`-Eigenschaft definiert die Rolle, die von den Membern in `memberInfos` übernommen wurde (z. B. SIGNER und APPROVER).

```
  POST /api/rest/v6/widgets HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  x-api-user: email:your-api-user@your-domain.com
  Content-Type: application/json

  Request Body:

  {
    "fileInfos": [
      {
      "transientDocumentId": "YOUR-TRANSIENT-DOCUMENT-ID"
      }
     ],
    "name": "Insurance Form",
      "widgetParticipantSetInfo": {
          "memberInfos": [{
              "email": ""
          }],
      "role": "SIGNER"
      },
      "state": "ACTIVE"
  }

  Response Body (example):

  {
     "id": "CBJ...PXoK2o"
  }
```

Sie können ein Webformular als `DRAFT` oder `AUTHORING` erstellen und dann seinen Status ändern, wenn das Formular Ihre Anwendungspipeline durchläuft. Zum Ändern des Status eines Webformulars verweisen Sie auf den Endpunkt [PUT /widgets/{widgetId}/state](https://secure.na4.adobesign.com/public/docs/restapi/v6#!/widgets/updateWidgetState).

## Webformular-Hosting-URL lesen

Der nächste Schritt besteht darin, die URL zu ermitteln, die das Webformular hostet. Der Endpunkt /widgets ruft eine Liste von Webformulardaten ab, einschließlich der gehosteten URL des Webformulars, das Sie an Ihre Benutzer weiterleiten, um Signaturen und andere Formulardaten zu erfassen.

Dieser Endpunkt gibt eine Liste zurück, sodass Sie das bestimmte Formular anhand seiner ID in `userWidgetList` suchen können, bevor Sie die URL abrufen, die das Webformular hostet:

```
  GET /api/rest/v6/widgets HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  Accept: application/json

  Response Body:

  {
    "userWidgetList": [
      {
        "id": "CBJCHB...FGf",
        "name": "Insurance Form",
        "groupId": "CBJCHB...W86",
        "javascript": "<script type='text/javascript' ...
        "modifiedDate": "2021-03-13T15:52:41Z",
        "status": "ACTIVE",
        "Url":
        "https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIB...Rag*",
        "hidden": false
      },
      {
        "id": "CBJCHB...I8_",
        "name": "Insurance Form",
        "groupId": "CBJCHBCAABAAyhgaehdJ9GTzvNRchxQEGH_H1ya0xW86",
        "javascript": "<script type='text/javascript' language='JavaScript'
        src='https://sec
        "modifiedDate": "2021-03-13T02:47:32Z",
        "status": "ACTIVE",
        "Url":
        "https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIB...AAB",
        "hidden": false
      },
      {
        "id": "CBJCHB...Wmc",
```

## Verwalten des Webformulars

Dieses Formular ist ein PDF-Dokument, das von Benutzern ausgefüllt werden kann. Sie müssen dem Bearbeiter des Formulars jedoch mitteilen, welche Felder die Benutzer ausfüllen müssen und wo sie sich im Dokument befinden:

![Screenshot des Versicherungsformulars mit einigen Feldern](assets/GSASAPI_1.png)

Im obigen Dokument werden die Felder noch nicht angezeigt. Sie werden hinzugefügt, während definiert wird, welche Felder die Informationen des Unterzeichners sowie deren Größe und Position erfassen.

Wechseln Sie nun zur Registerkarte [Webformulare](https://secure.na4.adobesign.com/public/agreements/#agreement_type=webform) auf der Seite &quot;Ihre Vereinbarungen&quot; und suchen Sie nach dem Formular, das Sie erstellt haben.

![Screenshot der Registerkarte &quot;Verwalten&quot; in Acrobat Sign](assets/GSASAPI_2.png)

![Screenshot der Registerkarte &quot;Verwalten&quot; in Acrobat Sign mit ausgewählten Webformularen](assets/GSASAPI_3.png)

Klicken Sie auf **Bearbeiten**, um die Dokumentbearbeitungsseite zu öffnen. Die verfügbaren vordefinierten Felder befinden sich auf der rechten Seite.

![Screenshot der Acrobat Sign-Formularerstellungsumgebung](assets/GSASAPI_4.png)

Der Editor ermöglicht es Ihnen, Text- und Signaturfelder per Drag &amp; Drop einzufügen. Nachdem Sie alle erforderlichen Felder hinzugefügt haben, können Sie sie skalieren und ausrichten, um das Formular zu optimieren. Klicken Sie abschließend auf **Speichern**, um das Formular zu erstellen.

![Screenshot der Acrobat Sign-Formularautorenumgebung mit hinzugefügten Formularfeldern](assets/GSASAPI_5.png)

## Senden eines Webformulars zum Signieren

Wenn Sie das Webformular fertig gestellt haben, müssen Sie es übermitteln, damit es ausgefüllt und signiert werden kann. Nach dem Speichern des Formulars können Sie die URL und den eingebetteten Code anzeigen und kopieren.

**Webformular-URL kopieren**: Verwenden Sie diese URL, um Benutzer zur Überprüfung und Signatur an eine gehostete Version dieser Vereinbarung zu senden. Beispiel:

[https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIBAA3...babw\*](https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIBAA3AAABLblqZhCndYscuKcDMPiVfQlpaGPb-5D7ebE9NUTQ6x6jK7PIs8HCtTzr3HOx8U6D5qqbabw*)

**Webformular-Einbettungscode kopieren**: Fügen Sie die Vereinbarung Ihrer Website hinzu, indem Sie diesen Code kopieren und in Ihren HTML einfügen.

Beispiel:

```
<iframe
src="https://secure.na4.adobesign.com/public/esignWidget?wid=CBFC
...yx8*&hosted=false" width="100%" height="100%" frameborder="0"
style="border: 0;
overflow: hidden; min-height: 500px; min-width: 600px;"></iframe>
```

![Screenshot des endgültigen Webformulars](assets/GSASAPI_6.png)

Wenn Ihre Benutzer auf die gehostete Version Ihres Formulars zugreifen, überprüfen sie das temporäre Dokument, das zuerst mit den Feldern hochgeladen wurde, die wie angegeben positioniert sind.

![Screenshot des endgültigen Webformulars](assets/GSASAPI_7.png)

Der Benutzer füllt dann die Felder aus und signiert das Formular.

![Screenshot des Benutzers, der das Signaturfeld auswählt](assets/GSASAPI_8.png)

Als Nächstes signiert der Benutzer das Dokument mit einer zuvor gespeicherten Signatur oder mit einer neuen.

![Screenshot des Signiererlebnisses](assets/GSASAPI_9.png)

![Screenshot der Signatur](assets/GSASAPI_10.png)

Wenn der Benutzer auf **Anwenden** klickt, weist Adobe ihn an, seine E-Mail-Adresse zu öffnen und die Signatur zu bestätigen. Die Signatur steht bis zum Eingang der Bestätigung aus.

![Screenshot von nur einem weiteren Schritt](assets/GSASAPI_11.png)

Diese Authentifizierung bietet zusätzliche Faktor-Authentifizierung und erhöht die Sicherheit des Unterzeichnungsprozesses.

![Screenshot der Bestätigungsmeldung](assets/GSASAPI_12.png)

![Screenshot der Abschlussmeldung](assets/GSASAPI_13.png)

## Lesen abgeschlossener Webformulare

Jetzt ist es an der Zeit, die Formulardaten zu erhalten, die Benutzer ausgefüllt haben. Der Endpunkt &quot;`/widgets/{widgetId}/formData`&quot; ruft Daten ab, die der Benutzer beim Signieren des Formulars in ein interaktives Formular eingegeben hat.

```
GET /api/rest/v6/widgets/{widgetId}/formData HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: text/csv
```

Der resultierende CSV-Datenstrom enthält Formulardaten.

```
Response Body:
"Agreement
name","completed","email","role","first","last","title","company","agreementId",
"email verified","web form signed/approved"
"Insurance Form","","myemail@email.com","SIGNER","John","Doe","My Job Title","My
Company Name","","","2021-03-07 19:32:59"
```

## Erstellen einer Vereinbarung

Alternativ zu Webformularen können Sie Vereinbarungen erstellen. In den folgenden Abschnitten werden einige einfache Schritte zum Verwalten von Vereinbarungen mithilfe der Sign-API veranschaulicht.

Wenn ein Dokument zum Signieren oder Genehmigen an bestimmte Empfänger gesendet wird, wird eine Vereinbarung erstellt. Sie können den Status und den Abschluss einer Vereinbarung mithilfe von APIs nachverfolgen.

Sie können eine Vereinbarung mit einem [temporären Dokument](https://helpx.adobe.com/de/sign/kb/how-to-send-an-agreement-through-REST-API.html), [Bibliotheksdokument](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/samples/send_using_library_doc.md) oder einer URL erstellen. In diesem Beispiel basiert die Vereinbarung auf `transientDocumentId`, genau wie das zuvor erstellte Webformular.

```
POST /api/rest/v6/agreements HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
x-api-user: email:your-api-user@your-domain.com
Content-Type: application/json
Accept: application/json
Request Body:
{
    "fileInfos": [
      {
      "transientDocumentId": "{transientDocumentId}"
      }
     ],
    "name": "{agreementName}",
    "participantSetsInfo": [
      {
      "memberInfos": [
          {
          "email": "{signerEmail}"
          }
        ],
        "order": 1,
        "role": "SIGNER"
      }
    ],
    "signatureType": "ESIGN",
    "state": "IN_PROCESS"
  }
```

In diesem Beispiel wird die Vereinbarung als WIRD_VERARBEITET erstellt. Sie können sie jedoch in einem von drei verschiedenen Status erstellen:

* ENTWURF: Die Vereinbarung wird vor dem Versand schrittweise erstellt.

* AUTHORING - Zum Hinzufügen oder Bearbeiten von Formularfeldern in der Vereinbarung

* WIRD_VERARBEITET - Vereinbarung sofort senden

Um einen Vereinbarungsstatus zu ändern, verwenden Sie den Endpunkt &quot;`PUT /agreements/{agreementId}/state`&quot;, um einen der folgenden zulässigen Statusübergänge auszuführen:

* ENTWURF ZUM AUTHORING

* AUTHORING to IN_PROCESS

* IN_PROCESS to CANCELLED

Die `participantSetsInfo`-Eigenschaft oben stellt E-Mails von Personen bereit, von denen erwartet wird, dass sie an der Vereinbarung teilnehmen, und welche Aktion sie ausführen (signieren, genehmigen, bestätigen usw.). Im obigen Beispiel gibt es nur einen Teilnehmer: den Unterzeichner. Schriftliche Signaturen sind auf vier pro Dokument beschränkt.

Im Gegensatz zu Webformularen versendet der Adobe beim Erstellen einer Vereinbarung diese automatisch zur Signatur. Der Endpunkt gibt den eindeutigen Bezeichner der Vereinbarung zurück.


```
  Response Body:

  {
     id (string): The unique identifier of the agreement
  }
```

## Abrufen von Informationen zu Vereinbarungsmitgliedern

Nachdem Sie eine Vereinbarung erstellt haben, können Sie den Endpunkt &quot;`/agreements/{agreementId}/members`&quot; verwenden, um Informationen über die Mitglieder der Vereinbarung abzurufen. Sie können beispielsweise überprüfen, ob ein Teilnehmer die Vereinbarung signiert hat.

```
GET /api/rest/v6/agreements/{agreementId}/members HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: application/json
```

Der resultierende JSON-Antwortkörper enthält Informationen über die Teilnehmer.

```
  Response Body:

  {
     "participantSets":[
        {
           "memberInfos":[
              {
                 "id":"CBJ...xvM",
                 "email":"participant@email.com",
                 "self":false,
                 "securityOption":{
                    "authenticationMethod":"NONE"
                 },
                 "name":"John Doe",
                 "status":"ACTIVE",
                 "createdDate":"2021-03-16T03:48:39Z",
                 "userId":"CBJ...vPv"
              }
           ],
           "id":"CBJ...81x",
           "role":"SIGNER",
           "status":"WAITING_FOR_MY_SIGNATURE",
           "order":1
        }
     ],
```

## Erinnerungen für Vereinbarungen senden

Abhängig von den Geschäftsregeln kann eine Frist verhindern, dass Teilnehmer die Vereinbarung nach einem bestimmten Datum unterzeichnen. Wenn die Vereinbarung ein Ablaufdatum hat, können Sie Teilnehmer daran erinnern, sobald dieses Datum näher rückt.

Basierend auf den Informationen der Vereinbarungsmitglieder, die Sie nach dem Aufruf des `/agreements/{agreementId}/members`-Endpunkts im letzten Abschnitt erhalten haben, können Sie E-Mail-Erinnerungen an alle Teilnehmer ausgeben, die die Vereinbarung noch nicht unterzeichnet haben.

Eine Vereinbarungsanforderung für den `/agreements/{agreementId}/reminders`-Endpunkt erstellt eine Erinnerung für die angegebenen Teilnehmer einer POST, die durch den `agreementId`-Parameter identifiziert wird.

```
POST /agreements/{agreementId}/reminders HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
x-api-user: email:your-api-user@your-domain.com
Content-Type: application/json
Accept: application/json
  Request Body:

  {
    "recipientParticipantIds": [{agreementMemberIdList}],
    "agreementId": "{agreementId}",
    "note": "This is a reminder that you haven't signed the agreement yet.",
    "status": "ACTIVE"
  }

  Response Body:

  {
     id (string, optional): An identifier of the reminder resource created on the
     server. If provided in POST or PUT, it will be ignored
  }
```

Sobald Sie die Erinnerung senden, erhalten die Benutzer eine E-Mail mit den Details der Vereinbarung und einem Link zur Vereinbarung.

![Screenshot der Erinnerungsnachricht](assets/GSASAPI_14.png)

## Abgeschlossene Vereinbarungen werden gelesen

Wie Webformulare können Sie Details zu Vereinbarungen lesen, die von den Empfängern signiert wurden. Der Endpunkt &quot;`/agreements/{agreementId}/formData`&quot; ruft die vom Benutzer beim Signieren des Webformulars eingegebenen Daten ab.

```
GET /api/rest/v6/agreements/{agreementId}/formData HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: text/csv
Response Body:
"completed","email","role","first","last","title","company","agreementId"
"2021-03-16 18:11:45","myemail@email.com","SIGNER","John","Doe","My Job Title","My
Company Name","CBJCHBCAABAA5Z84zy69q_Ilpuy5DzUAahVfcNZillDt"
```

## Nächste Schritte

Mit der Acrobat Sign-API können Sie Dokumente, Webformulare und Vereinbarungen verwalten. Die vereinfachten, aber vollständigen Workflows, die mit Webformularen und Vereinbarungen erstellt wurden, werden auf eine generische Weise ausgeführt, die es Entwicklern ermöglicht, sie mit jeder Sprache zu implementieren.

Eine Übersicht über die Funktionsweise der Sign-API finden Sie im [Entwicklerhandbuch zur API-Nutzung](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/api_usage.md). Diese Dokumentation enthält kurze Artikel zu vielen der im Artikel beschriebenen Schritte sowie weitere verwandte Themen.

Die Acrobat Sign-API ist über mehrere Ebenen von [Einzelbenutzer- und Mehrbenutzer-Abos für elektronische Signaturen](https://acrobat.adobe.com/de/de/sign/pricing/plans.html) verfügbar. Sie können also ein Preismodell wählen, das Ihren Anforderungen am besten entspricht. Da Sie nun wissen, wie einfach es ist, die Sign-API in Ihre Anwendungen zu integrieren, sind Sie möglicherweise an anderen Funktionen wie [Acrobat Sign Webhooks](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/webhooks.md), einem Push-basierten Programmiermodell, interessiert. Anstatt dass Ihre App häufige Überprüfungen in Acrobat Sign-Ereignissen durchführen muss, können Sie mit Webhooks eine HTTP-URL registrieren, für die die Sign-API bei jedem Auftreten eines POST eine Ereignisrückrufanforderung ausführt. Webhooks ermöglichen eine robuste Programmierung, indem sie Ihre Anwendung mit Echtzeit- und Sofortaktualisierungen versorgen.

Informieren Sie sich über das [Pay-as-you-go-Angebot](https://developer.adobe.com/document-services/pricing/main) für den Zeitpunkt, zu dem Ihre sechsmonatige kostenlose Adobe PDF Services-API-Testversion endet, sowie über die kostenlose Adobe PDF Embed-API.

Beginnen Sie mit [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html), um Ihrer App aufregende Funktionen wie automatische Dokumenterstellung und Dokumentsignierung hinzuzufügen.
