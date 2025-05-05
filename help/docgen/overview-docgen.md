---
title: API-Tutorials für die Dokumenterstellung
description: Übersicht über die Tutorials zur Dokumentenerzeugung-API
feature: Document Generation API
role: Developer
level: Beginner, Intermediate, Experienced
type: Tutorial
jira: KT-7480
thumbnail: KT-7480.jpg
exl-id: 519a41a2-33af-4022-8919-2cb69995c46c
source-git-commit: 0f62890207722245f33be41ad4c77ba68bf7a0fc
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---


# API-Tutorials zur Dokumenterstellung

Die Dokumentenerzeugung-API erstellt PDF- und Word-Dokumente aus Word-Vorlagen und JSON-Daten.

>[!NOTE]
>
>Die Dokumentenerzeugung-API ist in der PDF Services-API enthalten.

## Generieren von Dokumenten

<!-- Comment -->
<!-- CARDS

* https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen
  {target = _self}
  {title = Automate document generation}
  {description = Learn how to automatically generate documents at scale}
  {image = https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/media_120e532325e3fdc7f559ad9b446d9ec08c1e239a1.png?width=400&format=webply&optimize=medium}
  {cta = Watch}

-->
<!-- End Comment -->

<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Automate document generation">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" title="Automatisierte Dokumentenerstellung" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/media_120e532325e3fdc7f559ad9b446d9ec08c1e239a1.png?width=400&format=webply&optimize=medium" alt="Automatisierte Dokumentenerstellung"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" target="_self" rel="referrer" title="Automatisierte Dokumentenerstellung">Dokumenterstellung automatisieren</a>
                    </p>
                    <p class="is-size-6">Automatisches Generieren skalierter Dokumente.</p>
                </div>
                <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ansehen</span>
                3</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Erstellen von Vorlagen

Die API für die Dokumentenerzeugung akzeptiert eine Dokumentvorlage (mit Vorlagen-Tags) zusammen mit den Eingabedaten, um das endgültige Dokument zu generieren. Das endgültige Dokument wird generiert, indem alle Vorlagen-Tags in der Dokumentvorlage durch den dynamischen Inhalt ersetzt werden, der auf den tatsächlichen Werten basiert, die der Dateneingabe entsprechen.

<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Overview of the Adobe Document Generation Tagger">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" title="Überblick über das Adobe-Tagger zum Generieren von Dokumenten" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/media_17b19864efffdb1f8c54017812c7de662e17bf163.png?width=400&format=webply&optimize=medium" alt="Überblick über das Adobe-Tagger zum Generieren von Dokumenten"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" target="_self" rel="referrer" title="Überblick über das Adobe-Tagger zum Generieren von Dokumenten">Übersicht über das Adobe-Tag zum Generieren von Dokumenten</a>
                    </p>
                    <p class="is-size-6">Verschaffen Sie sich einen Überblick über den Adobe-Tagger für die Dokumentgenerierung, der für die Verwendung mit der Adobe-API für die Dokumentgenerierung entwickelt wurde.</p>
                </div>
                <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ansehen</span>
                3</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding text tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" title="Hinzufügen von Text-Tags" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/media_113bb0b6c3dfa1329810d3afbba3498af82c6c875.png?width=400&format=webply&optimize=medium" alt="Hinzufügen von Text-Tags"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" target="_self" rel="referrer" title="Hinzufügen von Text-Tags">Hinzufügen von Text-Tags</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie Text-Tags mit dem Tagger "Dokumentenerzeugung" in Microsoft Word-Vorlagen einfügen</p>
                </div>
                <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ansehen</span>
                3</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding image tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" title="Hinzufügen von Bild-Tags" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/media_1095ed3adad9c9360bb3184dccc41a72a3da94edc.png?width=400&format=webply&optimize=medium" alt="Hinzufügen von Bild-Tags"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" target="_self" rel="referrer" title="Hinzufügen von Bild-Tags">Hinzufügen von Bild-Tags</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie mit dem Adobe-Tagger "Dokumentenerzeugung" Bild-Tags zu Microsoft Word-Vorlagen hinzufügen, um Bilder dynamisch in Dokumente zu übertragen</p>
                </div>
                <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ansehen</span>
                3</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding tables and list tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" title="Hinzufügen von Tabellen und Listen-Tags" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/media_1c2cc8e4bf3a85a977104a3d94073c37b93dcfdf9.png?width=400&format=webply&optimize=medium" alt="Hinzufügen von Tabellen und Listen-Tags"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" target="_self" rel="referrer" title="Hinzufügen von Tabellen und Listen-Tags">Hinzufügen von Tabellen und Listen-Tags</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie Tabellen und Listen-Tags mit dem Adobe-Tagger zur Dokumentgenerierung zu Microsoft Word-Vorlagen hinzufügen, um Tabellen- oder Listenzeilen basierend auf Daten dynamisch hinzuzufügen.</p>
                </div>
                <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ansehen</span>
                3</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Setting numerical calculation tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" title="Festlegen von numerischen Berechnungstags" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/media_1a64d90689430bd8a1f7a272a66f006f0808ab6cf.png?width=400&format=webply&optimize=medium" alt="Festlegen von numerischen Berechnungstags"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" target="_self" rel="referrer" title="Festlegen von numerischen Berechnungstags">Festlegen von numerischen Berechnungs-Tags</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie numerische Berechnungs-Tags in Microsoft Word-Vorlagen mit dem Adobe-Tagger für die Dokumentgenerierung zum Berechnen von Aggregationen oder Arithmetik von Datenwerten festlegen</p>
                </div>
                <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ansehen</span>
                3</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Setting conditional content">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" title="Festlegen von bedingtem Inhalt" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/media_145cebc42cffee358ed1beffcd5015ecb595fc82a.png?width=400&format=webply&optimize=medium" alt="Festlegen von bedingtem Inhalt"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" target="_self" rel="referrer" title="Festlegen von bedingtem Inhalt">Bedingten Inhalt einstellen</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie Abschnitte in Microsoft Word-Vorlagen mit dem Adobe-Tagger für die Dokumentgenerierung festlegen, um Abschnitte eines Dokuments basierend auf Daten dynamisch ein- oder auszuschließen</p>
                </div>
                <a href="https://experienceleague.adobe.com/de/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ansehen</span>
                3</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
