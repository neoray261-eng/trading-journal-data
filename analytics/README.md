# Deep Analytics — Not Shown on Dashboard

תיקייה זו מכילה נתונים מתקדמים לניתוח פוסט-עסקה. **הלוח החי לא נוגע בקבצים כאן** — אלה נתונים לצפייה בעת ניתוח מכוון, לא במבט יומי.

## `trades_deep.json`

מתעדכן אוטומטית על-ידי `check_outcomes.yml` כל 15 דקות. לכל עסקה שנסגרה יש רשומה עם:

| שדה | משמעות |
|---|---|
| `signal_ts` | מתי האיתות נורה |
| `outcome_ts` | מתי היעד/סטופ נגעו |
| `bot`, `symbol`, `direction`, `killzone` | זיהוי בסיסי |
| `entry_price`, `stop_price`, `target_price` | המחירים המתוכננים |
| `outcome` | `target` / `stop` / `expired` |
| `outcome_price` | מחיר ההשקה (הסטופ או היעד) |
| `bar_extreme` | הקצה בפועל של הבר החוצה |
| `slippage_pts` | הפער בין המתוכנן לקצה בפועל |
| **`mfe_pts`** | Maximum Favorable Excursion — עד כמה המחיר הלך *לטובתך* במהלך העסקה |
| **`mae_pts`** | Maximum Adverse Excursion — עד כמה המחיר הלך *נגדך* לפני הסגירה |
| `mfe_r`, `mae_r` | אותם ערכים ביחידות R (יחסית לסיכון) |
| `mfe_time_min` | כמה דקות אחרי הכניסה הגיע ה-MFE |
| `mae_time_min` | כמה דקות אחרי הכניסה הגיע ה-MAE |
| `duration_min` | סה"כ זמן העסקה |

## איך אלה עוזרים בניתוח

**MFE גבוה + סטופ נפגע** → המחיר הלך יפה לטובה, ואז חזר. אולי:
- היעדים גרעיים מדי (הרבה זכיות פוטנציאליות "נגנבו")
- אין `trailing stop` — היינו יכולים לנעול חלק מהרווח

**MAE גבוה + יעד נפגע** → המחיר כמעט פגע בסטופ לפני שהתחיל לנוע נכון. אולי:
- הסטופים צרים מדי
- הרבה זכיות ניצלו בעור השן ואי-אפשר להסתמך על זה

**MFE נמוך + סטופ נפגע מהר** → הכניסה הייתה גרועה. הבוט זיהה setup במיקום לא נכון.

**Duration ארוך + יעד** → סבלנות משתלמת. Duration קצר + סטופ → כניסה שגויה.

## איך לצפות

הקובץ פשוט JSON:
```bash
curl -s https://raw.githubusercontent.com/neoray261-eng/trading-journal-data/main/analytics/trades_deep.json | jq
```

או בצ'אט: תגיד "בוא ננתח את trades_deep" ואני קורא ומסכם לך.
