# ALFA 3 | program pro práci s databází úřadu práce
- autor: Jiří Syrovátko
- email: syrovatko@spsejecna.cz
- datum vypracování: 4.2. 2024
- škola: SPŠE Ječná
- Jedná se o projekt školní.

## Využité technologie
- Python
- SQL
- SSMS - databáze vytvořená lokálně nebo na serveru

## Práce s programem
- uživatel pracuje s programem pomocí zadávání čísel z výběru
- u zadávání vlastností objektů databáze je za potřebí zadat požadovanou hodnotu
- uživatel v některých úkonech je nucen potvrdit svůj výběr

## Databáze
### Tabulky:
- Employer
- TypeOfJob
- AddressOfJob
- Job
- Customer
- Employee
- JobContract

![db diagram](https://github.com/JirkS/PV-database-Alfa/blob/main/EmploymentDepartmentDiagramRelational.png)



## Funcionalita
- Program má za úkol poskutovat práci a kontrolu pro databází úřadu práce.
- Na začátku program vypíše main menu, kde je CRUD pro praci s tabulkami, transakce, print report, improt dat z .csv souboru, restarovani databáze (smazání a vytvoření tabulek) a exit program.
- Pomocí zadávání čísel v daném rozmezí podle příkazů uživatel vybírá akce.
- Je mu dostáváná zpětná vazba a je pobízen k zadávání určitých hodnot potřebné pro vybraný příkaz.

## Instalace a spuštění
- pro úspěšné zapnutí programu je zapotřebí mít na zařízení nainstalovaný python
- stáhneme si zip soubor a extrahujeme ho
- spustíme si windows cmd
- dojdeme pomocí příslušných příkazů do složky projektu (DatabaseAlfa)
- poté program spustíme pomocí příkazu: "python main.py"
- musíte mít zárověn běžící databázi, v defaultním nastavení databáze běží na serveru autora a není potřeba nic měnit, jinak je třeba změnit nastavení databáze v config.ini ve složce config pro připojení do databáze, vytvořit databázi pomocí scriptu "scriptToCreateDB.txt" ve složce "doc/database/"

## Hlavní Třídy
### Compressor
- Třída obsahuje metody pro kompletní kompresi textového souboru.
#### metody:
- compress() - spouští všechny metody pro kompresi
- count_num_of_words() - spočítá všechny slova v textovém souboru
- fill_dic_by_the_most_common_words() - naplní list slovy s největší četností - využití frekvenční analýzi
- fill_dic_of_two_word_phrases() - naplní dictionary dvěma nejčastějšími slovními spojeními o dvou slovech
- fill_dic_of_abbreviation() - naplní dictionary slovy společně s jejich zkratkami podle frekvenční anlýzi
- delete_short_sentences() - smaže nepodstatné, nedůležité a krátké nicneřikající věty
- delete_useless_words() - smaže nicneříkající, výplňová slova
- replace_words_with_abbreviations() - vymění slova za zkratky
- replace_words_of_coupling_synonyms() - vymění slova se stejným významem za nejkratší možnost
- replace_words_with_abbreviations_from_dictionary() - vymění slova za zkratky, které se vybraly
- fill_best_of_rest_of_the_most_common_words() - naplní list zbylými, nejvíce častými slovy
- replace_words_for_symbols() - vymění slova za vygenerované symboly
- write_csv_file() - zapíše slovník do csv souboru
- check_directory_path() - zkontroluje správnost cesty
- write_final_text_file() - zapíše finání verzi zkomprimovaného souboru
- Tato třída generuje různé verze rozvrhů pomocí náhodného vybírání předmětů. Pravděpodobnost, že se budou jakékoliv předměty totžné, je velmi mizivá. Každopádně i tyto varianty jsou bezpečně ohlídané, aby k nim nedocházelo. Třída slouží převážně pro vygenerování co nejvíce různých rozvrhů a nezáleží na tom, jak moc jsou kvalitní. Jde zkrátko o to, vygenerovat jich co nejvíce.
### Loader
- Obsahuje metody pro načítání statických souborů ".json" a ".txt".
- Kontroluje správnost cest.
### Template
- Tato třída obsahuje listy a dictionary, keteré obsahují načtené hodnoty ze statických souborů ".json".
### Logger
- Tato třída slouží pro zapisování do log souborů (ingo.log a error.log ve složce log/) 
- Obsahuje metodu pro filtraci informací z informačního log souboru pro výpis před ukočením programu.

## Pravidla a způsob čtení zkomprimovaného textového souboru
- Textový soubor musí být napsán v jazyce výhradně českém s kompletní diakritikou.
- Zkomprimovaný soubor lze plně přečíst pomocí slovníků v output složce.

## Konfigurační soubor
- Všechno potřebné je nastaveno v souboru config.ini, který lze najít ve složce config/config.ini.
- Pro uživatele jsou k dispozici části "USER-STATIC" - základní nastavení.
### "[DB]":
- driver = {ODBC Driver 17 for SQL Server}
- server = 193.85.203.188
- database = syrovatko
- uid = syrovatko
- pwd = SqlJirk5
### "[USER-STATIC]":
- csv_import_data_file = zadejte cestu k .csv souboru, který chcete importovat. Defaultně je cesta nastavená na složku input/nazev_souboru.csv


