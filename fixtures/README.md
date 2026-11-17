# Messdatensatz

`emails.jsonl` enthält 30 handgeschriebene Anfragen, wie sie im Postfach eines
Restaurants realistisch eintreffen. Jeder Datensatz trägt ein Soll-Label,
damit die Genauigkeit des Klassifizierers messbar ist und nicht behauptet
werden muss.

| Feld | Bedeutung |
|---|---|
| `id` | stabile Kennung, wird in Eval-Berichten referenziert |
| `subject`, `body` | die Nachricht, wie sie ankommen würde |
| `label` | Soll-Wert: `reservierung`, `frage`, `gruppe`, `beschwerde`, `sonstiges` |
| `difficulty` | `easy` oder `hard` — die Genauigkeit wird für beide getrennt berichtet |
| `note` | warum dieser Fall knifflig ist |

Verteilung: Reservierung 10 · Frage 7 · Gruppe 5 · Beschwerde 5 · Sonstiges 3
(17 einfach / 13 schwer).

## Warum eine fünfte Kategorie

Die Projektbeschreibung nennt vier Kategorien. Ein echtes Postfach bekommt
zusätzlich Lieferantenrechnungen, Vertriebs-Kaltakquise und Bewerbungen
(`e28`–`e30`). Ohne eine Klasse `sonstiges` wäre der Agent gezwungen, diese
falsch einzuordnen, und würde zum Beispiel einem Vertriebler antworten, als
wäre er ein Gast. Das fünfte Label hält die Messung ehrlich und gibt dem
Workflow einen Pfad „an einen Menschen weiterleiten, nicht antworten".

## Bewusst eingebaute Grenzfälle

- `e01` / `e10` — dieselbe Buchung zweimal geschickt; die Doppelbuchungsprüfung
  muss die zweite abfangen.
- `e04` — eine Absage, keine Neubuchung. Darf keine Zeile anlegen.
- `e09` — genau 8 Gäste, die Grenze der Regel „Gruppen über 8 brauchen Freigabe".
- `e05`, `e08` — eine Buchung mit eingebetteter Frage; die Buchung ist die Absicht.
- `e26`, `e27` — Beschwerden, so höflich formuliert, dass sie wie Fragen klingen.
