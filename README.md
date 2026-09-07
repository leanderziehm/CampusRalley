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
