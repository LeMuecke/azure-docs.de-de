---
title: 'Schnellstart: Testen der Wissensdatenbank mit Batchfragen'
description: Verwenden Sie das QnA Maker-Batchtesttool, um die Wissensdatenbanken in Ihrer QnA Maker-Ressource auf erwartete Antworten, Konfidenzscores und Eingabeaufforderungen mit mehreren Durchläufen.
ms.topic: tutorial
ms.date: 05/26/2020
ms.openlocfilehash: 01c767a6fbea5061f644052234f7462857dd342b
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84337732"
---
# <a name="quickstart-test-knowledge-base-with-batch-questions-and-expected-answers"></a>Schnellstart: Testen der Wissensdatenbank mit Batchfragen und erwarteten Antworten

Verwenden Sie das QnA Maker-Batchtesttool, um die Wissensdatenbanken in Ihrer QnA Maker-Ressource auf erwartete Antworten, Konfidenzscores und Eingabeaufforderungen mit mehreren Durchläufen.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Erstellen Sie einen QnA Maker-Dienst](create-publish-knowledge-base.md), oder verwenden Sie einen vorhandenen Dienst, der Englisch als Sprache verwendet.
* Laden Sie die [`.docx`-Datei mit einem Beispiel für mehrere Durchläufe](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx) herunter.
* Laden Sie das [Batchtesttool](https://aka.ms/qnamakerbatchtestingtool) herunter, und extrahieren Sie die ausführbare Datei aus der `.zip`-Datei.

## <a name="sign-into-qna-maker-portal"></a>Anmelden beim QnA Maker-Portal

[Melden](https://www.qnamaker.ai/) Sie sich beim QnA Maker-Portal an.

## <a name="create-a-new-knowledge-base-from-the-multi-turn-sampledocx-file"></a>Erstellen Sie eine neue Wissensdatenbank aus der .docx-Beispieldatei für mehrere Durchläufe.

1. Wählen Sie aus der Symbolleiste die Option **Wissensdatenbank erstellen** aus.
1. Überspringen Sie **Schritt 1**, da Sie bereits über eine QnA Maker-Ressource verfügen, und fahren Sie mit **Schritt 2** fort, um die Informationen zu Ihrer vorhandenen Ressource auszuwählen:
    * Azure Active Directory-ID
    * Name des Azure-Abonnements
    * Name des Azure QnA-Diensts
    * Sprache: Englisch
1. Geben Sie `Multi-turn batch test quickstart` als Namen Ihrer Wissensdatenbank ein.

1. Konfigurieren Sie in **Schritt 4** die Einstellungen anhand der folgenden Tabelle:

    |Einstellung|Wert|
    |--|--|
    |**Enable multi-turn extraction from URLs, .pdf or .docx files** (Mehrfachdurchlauf-Extrahierung von URLs, PDF- oder DOCX-Dateien aktivieren)|Aktiviert|
    |**Default answer text** (Standardantworttext)| `Batch test - default answer not found.`|
    |**+ Datei hinzufügen**|Wählen Sie die heruntergeladene `.docx`-Datei aus, die in den Voraussetzungen aufgeführt ist.|
    |**Smalltalk**|Wählen Sie **Professional** aus.|

1. Wählen Sie in **Schritt 5** die Option **Wissensdatenbank erstellen** aus.

    Wenn der Erstellungsprozess beendet ist, zeigt das Portal die bearbeitbare Wissensdatenbank an.

## <a name="save-train-and-publish-knowledge-base"></a>Speichern, Trainieren und Veröffentlichen der Wissensdatenbank

1. Wählen Sie in der Symbolleiste **Speichern und trainieren** aus, um die Wissensdatenbank zu speichern.
1. Wählen Sie in der Symbolleiste die Option **Veröffentlichen** aus, und wählen Sie dann erneut **Veröffentlichen** aus, um die Wissensdatenbank zu veröffentlichen. Durch Veröffentlichen wird die Wissensdatenbank für Abfragen von einem öffentlichen URL-Endpunkt zur Verfügung gestellt. Wenn die Veröffentlichung abgeschlossen ist, speichern, Sie die Informationen zu Host-URL und Endpunktschlüssel, die Sie auf der Seite **Veröffentlichen** finden.

    |Erforderliche Daten| Beispiel|
    |--|--|
    |Veröffentlichter Host|`https://YOUR-RESOURCE-NAME.azurewebsites.net`|
    |Veröffentlichter Schlüssel|`XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX` (Zeichenfolge aus 32 Zeichen, die nach `Endpoint` angezeigt wird)|
    |App-ID|`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` (Zeichenfolge aus 36 Zeichen, die als Teil von `POST` angezeigt wird) |

## <a name="create-batch-test-file-with-question-ids"></a>Erstellen einer Batchtestdatei mit Fragen-IDs

Um das Batchtesttool zu verwenden, erstellen Sie mit einem Text-Editor eine Datei namens `batch-test-data-1.tsv`. Die Datei muss folgende durch Tabulatorzeichen getrennt Spalten aufweisen.

|Felder der TSV-Eingabedatei|Notizen|Beispiel|
|--|--|--|
|Wissensdatenbank-ID|Die ID Ihrer Wissensdatenbank, die Sie auf der Seite „Veröffentlichen“ finden. Sie können verschiedene Wissensdatenbanken gleichzeitig in einer einzigen Datei testen, indem Sie mehrere Wissensdatenbank-IDs in einer Datei verwenden.|`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` (Zeichenfolge aus 36 Zeichen, die als Teil von `POST` angezeigt wird) |
|Frage|Den Text einer Frage, die ein Benutzer eingeben würde. Max. 1.000 Zeichen.|`How do I sign out?`|
|Metadatentags|optional|`topic:power` verwendet das Format _Schlüssel:Wert_|
|Oberster Parameter|optional|`25`|
|Erwartete Antwort-ID|optional|`13`|

Fügen Sie für diese Wissensdatenbank drei Zeilen nur der erforderlichen beiden Spalten zur Datei hinzu. Die erste Spalte enthält die ID Ihrer Wissensdatenbank, die zweite besteht aus der folgenden Liste mit Fragen:

|Spalte 2: Fragen|
|--|
|`Use Windows Hello to sign in`|
|`Charge your Surface Pro 4`|
|`Get to know Windows 10`|

Diese Fragen sind die genaue Formulierung aus der Wissensdatenbank und sollten einen Konfidenzscore von 100 zurückgeben.

Fügen Sie in drei weiteren Zeilen einige weitere Fragen hinzu, die den vorhandenen Fragen ähneln, aber nicht genau mit ihnen übereinstimmen. Verwenden Sie dieselbe Wissensdatenbank-ID:

|Spalte 2: Fragen|
|--|
|`What is Windows Hello?`|
|`How do I charge the laptop?`|
|`What features are in Windows 10?`|

> [!CAUTION]
> Stellen Sie sicher, dass die Spalten nur durch ein Tabulatortrennzeichen getrennt sind. Führende oder nachstehende Leerzeichen werden den Spaltendaten hinzugefügt und führen dazu, dass das Programm Ausnahmen auslöst, wenn Typ oder Größe falsch sind.

Die Batchtestdatei sieht beim Öffnen in Excel etwa wie die folgende Abbildung aus. Aus Sicherheitsgründen wurde die Wissensdatenbank-ID durch `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` ersetzt. Stellen Sie bei Ihrem eigenen Batchtest sicher, dass die Spalte die ID Ihrer Wissensdatenbank anzeigt.

> [!div class="mx-imgBorder"]
> ![Erste Version der TSV-Eingabedatei aus dem Batchtest](../media/batch-test/batch-test-1-input.png)

## <a name="test-the-batch-file"></a>Testen der Batchdatei

Führen Sie das Batchtestprogramm im folgenden CLI-Format an der Befehlszeile aus.

Ersetzen Sie `YOUR-RESOURCE-NAME` und `ENDPOINT-KEY` durch Ihre eigenen Werte für Dienstname und Endpunktschlüssel. Sie finden diese Werte auf der Seite **Einstellungen** im QnA Maker-Portal.

```console
batchtesting.exe batch-test-data-1.tsv https://YOUR-RESOURCE-NAME.azurewebsites.net ENDPOINT-KEY out.tsv
```
Der Test wird abgeschlossen und generiert die Datei `out.tsv`:

> [!div class="mx-imgBorder"]
> ![Erste Version der TSV-Ausgabedatei aus dem Batchtest](../media/batch-test/batch-test-1-output.png)

Aus Sicherheitsgründen wurde die Wissensdatenbank-ID durch `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` ersetzt. Bei Ihrem eigenen Batchtest zeigt die Spalte die ID Ihrer Wissensdatenbank an.

Die Testausgabe des Konfidenzscores (in der 4. Spalte) zeigt, dass für die drei oberen Fragen ein Wert von 100 zurückgegeben wurde. Dies war zu erwarten, weil jede Frage genau so lautet, wie sie in der Wissensdatenbank vorhanden ist. Die drei unteren Fragen, die jeweils neu formuliert wurden, geben als Konfidenzscore nicht 100 zurück. Um den Score sowohl für den Test als auch für Ihre Benutzer zu erhöhen, müssen Sie weitere alternative Fragen zur Wissensdatenbank hinzufügen.

## <a name="testing-with-the-optional-fields"></a>Testen mit optionalen Feldern

Sobald Sie das Format und den Prozess genau kennen, können Sie eine Testdatei generieren und diese mit einer Datenquelle wie beispielsweise Chatprotokollen in Ihrer Wissensdatenbank ausführen.

Da Datenquelle und Prozess automatisiert sind, können Sie die Testdatei mehrmals mit verschiedenen Einstellungen ausführen, um die richtigen Werte zu ermitteln.

Ein Beispiel: Wenn Sie über ein Chatprotokoll verfügen und ermitteln möchten, welcher Text dieses Protokolls auf welche Metadatenfelder angewendet wird, erstellen Sie eine Testdatei, und legen Sie die Metadatenfelder für jede Zeile fest. Führen Sie den Test aus, und überprüfen Sie die Zeilen, die mit den Metadaten übereinstimmen. Im Allgemeinen sollten die Treffer positiv sein, aber Sie sollten die Ergebnisse auf False Positives überprüfen. Ein False Positive ist eine Zeile, die mit den Metadaten übereinstimmt, aber für den jeweiligen Text nicht damit übereinstimmen sollte.

## <a name="using-optional-fields-in-the-input-batch-test-file"></a>Verwenden von optionalen Feldern in der Batchtestdatei für die Eingabe

Verwenden Sie das folgende Diagramm, um zu ermitteln, wie Sie die Feldwerte für optionale Daten herausfinden.

|Spaltennummer|Optionale Spalte|Speicherort der Daten|
|--|--|--|
|3|metadata|Exportieren Sie vorhandene _Schlüssel:Wert_-Paare aus der vorhandenen Wissensdatenbank.|
|4|top|Ein Standardwert von `25` wird empfohlen.|
|5|ID des Satzes aus Frage und-Antwort|Exportieren Sie ID-Werte aus der vorhandenen Wissensdatenbank. Beachten Sie auch die IDs, die in der Ausgabedatei zurückgegeben wurden.|

## <a name="add-metadata-to-the-knowledge-base"></a>Hinzufügen von Metadaten zur Wissensdatenbank

1. Fügen Sie im QnA-Portal auf der Seite **Bearbeiten** Metadaten aus `topic:power` zu den folgenden Fragen hinzu:

    |Fragen|
    |--|
    |Charge your Surface Pro 4|
    |Check the battery level|

    Für zwei Frage-Antwort-Paare ist der Metadatensatz vorhanden.

    > [!TIP]
    > Um die Metadaten und QnA-IDs der einzelnen Sätze anzuzeigen, exportieren Sie die Wissensdatenbank. Wechseln Sie zur Seite **Einstellungen**, und wählen Sie für den **Export** das Dateiformat `.xls` aus. Suchen Sie diese heruntergeladene Datei, öffnen Sie sie in Excel, und überprüfen Sie Metadaten und ID.

1. Klicken Sie auf **Speichern und trainieren**, wechseln Sie zur Seite **Veröffentlichen**, und klicken Sie dann auf die Schaltfläche **Veröffentlichen**. Diese Aktionen stellen dem Batchtest die Änderungen zur Verfügung. Laden Sie die Wissensdatenbank von der Seite **Einstellungen** herunter.

    Die heruntergeladene Datei weist das korrekte Format für die Metadaten und die richtige ID für den Satz aus Fragen und Antworten auf. Verwenden Sie diese Felder im nächsten Abschnitt.

    > [!div class="mx-imgBorder"]
    > ![Exportierte Wissensdatenbank mit Metadaten](../media/batch-test/exported-knowledge-base-with-metadata.png)

## <a name="create-a-second-batch-test"></a>Erstellen eines zweiten Batchtests

Bei Batchtests gibt es zwei Hauptszenarien:
* **Verarbeiten von Chatprotokolldateien**: Sie möchten die häufigste Antwort für eine bisher unbekannte Frage ermitteln. Dieses Szenario tritt am häufigsten dann auf, wenn Sie eine Protokolldatei mit Fragen verarbeiten müssen, beispielsweise Benutzerfragen an einen Chatbot. Erstellen Sie einen Batchdateitest, und verwenden Sie dabei nur die erforderlichen Spalten. Der Test gibt die häufigste Antwort für jede Frage zurück. Das bedeutet nicht, dass diese Antwort auch die richtige Antwort ist. Sobald Sie den Test abgeschlossen haben, fahren Sie mit dem Validierungstest fort.
* **Validierungstest**: Überprüfen Sie die erwartete Antwort. Für diesen Test müssen alle Fragen und die entsprechenden erwarteten Antworten im Batchtest überprüft worden sein. Hierfür ist ein gewisses Maß an manueller Verarbeitung erforderlich.

Beim folgenden Verfahren wird davon ausgegangen, dass Chatprotokolle verarbeitet werden sollen.

1. Erstellen Sie eine neue Batchtestdatei, der optionale Daten hinzugefügt werden können: `batch-test-data-2.tsv`. Fügen Sie die sechs Zeilen aus der ursprünglichen Eingabedatei für den Batchtest hinzu, und fügen Sie dann die Metadaten, die häufigste Antwort und die ID des Frage-Antwort-Paars für jede Zeile hinzu.

    Um den automatisierten Prozess der Überprüfung neuer Texte aus Chatprotokollen anhand der Wissensdatenbank zu simulieren, legen Sie die Metadaten für jede Spalte auf den gleichen Wert fest: `topic:power`.

    > [!div class="mx-imgBorder"]
    > ![Zweite Version der TSV-Eingabedatei aus dem Batchtest](../media/batch-test/batch-test-2-input.png)

1. Führen Sie den Test erneut aus, und ändern Sie dabei die Namen der Ein- und Ausgabedatei so, dass erkennbar ist, dass es sich um den zweiten Test handelt.

    > [!div class="mx-imgBorder"]
    > ![Zweite Version der TSV-Ausgabedatei aus dem Batchtest](../media/batch-test/batch-test-2-output.png)

## <a name="test-results-and-an-automated-test-system"></a>Testergebnisse und automatisiertes Testsystem

Diese Testausgabedatei kann im Rahmen einer automatisierten kontinuierlichen Testpipeline analysiert werden.

Diese spezifische Testausgabe kann folgendermaßen gelesen werden: Jede Zeile wurde mit Metadaten gefiltert, und weil nicht jede Zeile mit den Metadaten in der Wissensdatenbank übereinstimmte, wurde folgende Standardantwort für diese nicht übereinstimmenden Zeilen zurückgegeben: „No good match found in KB“. Bei den Zeilen, bei denen eine Übereinstimmung vorhanden war, wurden QnA-ID und Score zurückgegeben.

Alle Zeilen gaben als Beschriftung „Incorrect“ zurück, weil keine Zeile mit der erwarteten Antwort-ID übereinstimmte.

Diesen Ergebnissen können Sie entnehmen, welche Texte eines Chatprotokolls Sie als Fragen in den verschiedenen Zeilen verwenden können. Auch wenn Sie bisher nichts über die Daten wussten, geben Ihnen die Ergebnisse viele Informationen zu den Daten, die Sie dann in Zukunft verwenden können:

* Metadaten
* QnA-ID
* Ergebnis Ihrer App

War die Filterung mit Metadaten eine gute Idee für diesen Test? Ja und Nein. Das Testsystem sollte Testdateien für jedes Metadatenpaar sowie einen Test ohne Metadatenpaare erstellen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Wissensdatenbank nicht weiter testen möchten, löschen Sie das Batchdateitool und die Testdateien.

Wenn Sie die Wissensdatenbank nicht weiterverwenden möchten, löschen Sie sie wie folgt:

1. Wählen Sie im QnA Maker-Portal im oberen Menü die Option **Meine Wissensdatenbanken**.
1. Klicken Sie in der Liste der Wissensdatenbanken in der Zeile der Wissensdatenbank dieser Schnellstartanleitung auf das Symbol **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz für QnA Maker (V4)](https://go.microsoft.com/fwlink/?linkid=2092179)
