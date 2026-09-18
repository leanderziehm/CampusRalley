# THD Campus Rallye Frontend

Static frontend for the THD Campus Rallye. The application is implemented in `index.html` and uses the assets and styles in this repository.

## How it works

- The frontend loads quiz data from the Campus Rallye backend.
- German data is loaded from `/de`; English data is loaded from `/en`.
- The backend URL is configured in `index.html` through `linkBase`.
- Progress and answers are stored in the browser's `localStorage`.
- The final page displays the solution word and the configured THD website link.

## Run locally

The app should be served through a local web server because it loads JavaScript and API data. From the frontend root, for example:

```powershell
python -m http.server 8080
```

Then open <http://localhost:8080> in a browser.

The backend must be reachable at the URL configured in `index.html`.

## Language selection

The default language is German. English can be selected with the URL parameter:

```text
http://localhost:8080/?lang=en
```

## Google Sheet data format

The backend converts the Google Sheet rows into question objects. A normal text question does not need a `type` field:

```text
question_0_text       What is the answer?
question_0_answer     Example
question_0_letter     A
question_0_correctText Correct!
```

A single multiple-choice question uses pipe-separated options. Multiple options can be correct:

```text
question_1_type       multiple-choice
question_1_text       Select all correct answers.
question_1_options    Option A|Option B|Option C
question_1_answer     Option A|Option C
question_1_letter     B
question_1_correctText Correct!
```

A `multiple-choice-input` question shows several empty input fields. The
possible answers are not displayed; they are only used internally to validate
the entered answers. The number of input fields equals the number of correct
answers. Multiple answers are separated with `|`:

```text
question_2_type       multiple-choice-input
question_2_text       Which materials were processed with a laser?
question_2_options    Wood|Plastic|Felt|Cork|Metal
question_2_answer     Wood|Plastic|Felt|Cork
question_2_letter     F
question_2_correctText Well observed!
```

The answer order does not matter, but duplicate answers are not accepted.

A grouped multiple-choice question requires every subquestion to be answered correctly before its shared letter is awarded:

```text
question_10_type                         multiple-choice-group
question_10_text                         Allgemeine Infos
question_10_subquestion_1_text           Wie viele Studierende gibt es aktuell an der THD?
question_10_subquestion_1_options        ca. 5.100|ca. 7.500|ca. 9.600|ca. 12.000
question_10_subquestion_1_answer         ca. 9.600
question_10_subquestion_2_text           Und wie viele Partnerhochschulen hat die THD weltweit?
question_10_subquestion_2_options        ca. 80|ca. 150|ca. 220|ca. 380
question_10_subquestion_2_answer         ca. 220
question_10_letter                       Z
question_10_correctText                   Beide Antworten sind richtig!
```

Subquestions can also use `multiple-choice-input` for several self-entered
answers. The `options` field remains hidden and is only used for validation.
Normal text subquestions use `text` and `answer`; multiple-choice subquestions
also use `type` and `options`. All three types can be mixed in one group:

```text
question_11_type                         multiple-choice-group
question_11_text                         Campus-Rallye
question_11_subquestion_1_type           multiple-choice
question_11_subquestion_1_text           Welche Farbe hat das Logo?
question_11_subquestion_1_options        Blau|Rot|Grün
question_11_subquestion_1_answer         Blau
question_11_subquestion_2_type           multiple-choice-input
question_11_subquestion_2_text           Welche Farben hat das Gebäude?
question_11_subquestion_2_options        Blau|Rot|Grün
question_11_subquestion_2_answer         Blau|Grün
question_11_letter                       C
question_11_correctText                   Beide Antworten sind richtig!
```

Question index `10` is displayed as question 11 because indexes start at zero.

## Deployment

The frontend is deployed through the GitHub repository:

```text
https://github.com/leanderziehm/CampusRalley
```

Push changes to `main` to trigger the connected Vercel frontend deployment:

```powershell
git add index.html styles.css README.md
git commit -m "Update frontend"
git push origin main
```

The backend is maintained and deployed separately from the `CampusRallyeBackend` repository.

### Neue Personen als Developer hinzufügen

Nur Personen mit Administratorrechten können neue Personen zum Repository hinzufügen.

1. Öffne das Repository auf GitHub.
2. Wähle **Settings** und anschließend **Collaborators** beziehungsweise **Collaborators and teams**.
3. Klicke auf **Add people** oder **Add people to this repository**.
4. Suche nach dem GitHub-Benutzernamen oder der E-Mail-Adresse der Person und sende die Einladung.
5. Wähle als Berechtigungsstufe **Write**. Diese Rolle entspricht dem normalen Developer-Zugriff: Die Person kann Branches und Commits pushen sowie Pull Requests erstellen, aber keine Repository-Einstellungen verwalten.
6. Falls in der Organisation eine eigene Rolle **Developer** angeboten wird, kann stattdessen diese Rolle ausgewählt werden.

Die eingeladene Person muss die Einladung zunächst über GitHub oder per E-Mail annehmen. Anschließend kann sie das Repository klonen und mit dem oben beschriebenen Branch- und Pull-Request-Ablauf arbeiten.
