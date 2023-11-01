# Tabellen in LiveLabs

## Tische sind cool!

Sie können eine Tabelle in Markdown wie folgt definieren:

    | Tables        | Are           | Cool  |
    | ------------- |:-------------:| -----:|
    | **col 3 is**  | right-aligned | $1600 |
    | col 2 is      | *centered*    |   $12 |
    | zebra stripes | ~~are neat~~  |    $1 |
    

Das Ergebnis sieht folgendermaßen aus:

| Tabellen | Sind | Cool |
| --- | :-: | --: |
| **Spalte 3 ist** | rechtsbündig | $1600 |
| Spalte 2 ist | _Zentriert_ | $12 |
| Zebrastreifen | sind ordentlich | $1 |

Sie können sehen, dass eine Standardtabellenbeschriftung bereitgestellt wird, die standardmäßig eine Verkettung des Workshop-Titels und des Labortitels ist.

Wenn Ihnen der Standardwert nicht gefällt, können Sie auch einen eigenen Tabellentitel angeben, indem Sie die folgende Tabellendefinition hinzufügen:

    {: title="My table title"}
    

Die vollständige Preisabschrift sieht folgendermaßen aus:

    | Tables        | Are           | Cool  |
    | ------------- |:-------------:| -----:|
    | **col 3 is**  | right-aligned | $1600 |
    | col 2 is      | *centered*    |   $12 |
    | zebra stripes | ~~are neat~~  |    $1 |
    {: title="My table title"}
    

Jetzt sieht unser Tisch so aus:

| Tabellen | Sind | Cool |
| --- | :-: | --: |
| **Spalte 3 ist** | rechtsbündig | $1600 |
| Spalte 2 ist | _Zentriert_ | $12 |
| Zebrastreifen | sind ordentlich | $1 |
| {: title="Mein Tabellentitel"} |  |  |

Wie Sie sehen, wird die Nummerierung automatisch hinzugefügt.

Ist das nicht cool?

Sie können auch das [LiveLabs Markdown Cheatsheet](https://objectstorage.us-ashburn-1.oraclecloud.com/p/MKKRgodQ0WIIgL_R3QCgCRWCg30g22bXgxCdMk3YeKClB1238ZJXdau_Jsri0nzP/n/c4u04/b/qa-form/o/LiveLabs_MD_Cheat_Sheet.pdf) lesen.