# Attempts log files

Elk bericht dat via SMTPeter is verstuurd is gelogd in bestanden met prefix
"attempts". Je kunt de inhoud van deze bestanden downloaden in CSV, JSON
en XML formaat via de [REST logfiles API](rest-logfiles) of via het dashboard.
Voor elke attempt wordt de volgende informatie opgeslagen:

| Naam       | Beschrijving                                                                                                                               |
|------------|------------------------------------------------------------------------------------------------------------------------------------------- |
| id         | De unieke ID van het verzonden bericht                                                                                                     |
| time       | Het tijdstip wanneer het bericht bij ons ontvangen was in het formaat JJJJ-MM-DD uu:mm:ss                                                  |
| envelope   | Het envelope adres van het bericht                                                                                                         |
| recipient  | De recipient van het bericht                                                                                                               |
| properties | De eigenschappen (preventscam, inlinecss, trackbounces, trackopens, and trackclicks) van het bericht (puntkomma's gescheiden)              |
| tags       | De tags van het bericht (puntkomma's gescheiden)                                                                                           |
| templateid | De ID van de template die gebruikt is voor het verstruurde bericht (0 als er geen template gebruikt is of wanneer dit niet beschikbaar is) |

## Meer informatie

* [REST calls niet gerelateerd aan verzending](./rest-other-calls)
* [REST events opvragen](./rest-events)
