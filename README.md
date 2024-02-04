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

- Ujistěte se že databáze vypadá takto:
  
![structure of db](https://github.com/JirkS/PV-database-Alfa/blob/main/Screenshot%202024-02-04%20215535.jpg)

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
- musíte mít zárověn běžící databázi, v defaultním nastavení databáze běží na serveru autora a není potřeba nic měnit, jinak je třeba změnit nastavení databáze v config.ini ve složce config pro připojení do databáze, vytvořit databázi na localu pomocí scriptu v SSMS "scriptToCreateDB.txt" ve složce "doc/database/"

## Návrhové vzory
- DAO - pro všechny tabulky databáze je vytvořená třída, která obsahuje CRUD - 4 základní operace pro práci s tabulkou: SELECT, INSERT, UPDATE, DELETE
- MVC - program je postavený na architektonickém vrozu: model - view - controller

## Struktura poragramu
1. CRUD - základní práce s tabulkami, po výběru si vybereze ze 4 ackcí s db a poté si vyberete s jako tabulkou chcete daný úkon provádět
2. Transaction salary based on Job from Employer to Customer - po zadání jste tázán pro zadání id nebo emailu employera a poté pro customera, poté se provede transakce peněz mezi zamestnavatelem a zakazníkem
3. Print report - vypíše 2 souhrnné reporty pro databázi
4. Import data from csv file - importuje data z .csv souboru nastaveném v konfiguračním souboru config.ini !!! Data musí být v .csv souboru zaznamenána ve správném formátu, příklad je uveden ve složce doc/CsvFileFormatDataToImportExample.csv
5. Restart db - restartuje databázi (smaže všechny tabulky a opětovně je vytvoří)
6. Exit the program - ukončí porgram

## Hlavní Třídy

### 1. Type_of_job
- **Atributy:**
  - `title`: str
  - `job_description`: str
  - `employment_type`: str
- **Metody:**
  - `__init__(self, title: str, job_description: str, employment_type: str)`

### 2. Address_of_job
- **Atributy:**
  - `country`: str
  - `city`: str
  - `street`: str
  - `postal_code`: int
- **Metody:**
  - `__init__(self, country: str, city: str, street: str, postal_code: int)`

### 3. Job
- **Atributy:**
  - `employer_id`: int
  - `type_of_job_id`: int
  - `address_of_job_id`: int
  - `salary`: float
  - `is_active`: bool
- **Metody:**
  - `__init__(self, employer_id: int, type_of_job_id: int, address_of_job_id: int, salary: float, is_active: bool)`

### 4. Customer
- **Atributy:**
  - `first_name`: str
  - `surname`: str
  - `email`: str
  - `phone`: str
  - `amount_of_money`: float
- **Metody:**
  - `__init__(self, first_name: str, surname: str, email: str, phone: str, amount_of_money: float)`

### 5. Employee
- **Atributy:**
  - `first_name`: str
  - `surname`: str
  - `email`: str
  - `phone`: str
  - `birth_date`: str
  - `is_active`: bool
- **Metody:**
  - `__init__(self, first_name: str, surname: str, email: str, phone: str, birth_date: str, is_active: bool)`

### 6. Job_contract
- **Atributy:**
  - `job_id`: int
  - `customer_id`: int
  - `employee_id`: int
  - `starting_date`: str
  - `ending_date`: str
- **Metody:**
  - `__init__(self, job_id: int, customer_id: int, employee_id: int, starting_date: str, ending_date: str)`

### 7. Type_of_jobDAO
- **Metody:**
  - `__init__(self, database: Database)`
  - `insert(self, type_of_job: Type_of_job)`
  - `select(self) -> List[Type_of_job]`
  - `delete(self, value: (int, str))`
  - `update(self, value: (int, str), job_description: str)`

### 8. Address_of_jobDAO
- **Metody:**
  - `__init__(self, database: Database)`
  - `insert(self, address_of_job: Address_of_job)`
  - `select(self) -> List[Address_of_job]`
  - `delete(self, value: str)`
  - `update(self, value: (int, str), street: str)`

### 9. JobDAO
- **Metody:**
  - `__init__(self, database: Database)`
  - `insert(self, job: Job)`
  - `select(self) -> List[Job]`
  - `delete(self, value: (str, int))`
  - `update(self, value: (int, str), salary: str)`

### 10. CustomerDAO
- **Metody:**
  - `__init__(self, database: Database)`
  - `insert(self, customer: Customer)`
  - `select(self) -> List[Customer]`
  - `delete(self, value: (int, str))`
  - `update(self, value: (int, str), amount_of_money: str)`

### 11. EmployeeDAO
- **Metody:**
  - `__init__(self, database: Database)`
  - `insert(self, employee: Employee)`
  - `select(self) -> List[Employee]`
  - `delete(self, value: (int, str))`
  - `update(self, value: (int, str), is_active: (str, int))`

### 12. Job_contractDAO
- **Metody:**
  - `__init__(self, database: Database)`
  - `insert(self, job_contract: Job_contract)`
  - `select(self) -> List[Job_contract]`
  - `delete(self, value: (str, int))`
  - `update(self, value: (int, str), set_ending_date: (date, str))`
 
### Regex Handler
- třída obsahuje metody pro kontrolu vstupních hodnot ponocí regexů
- **Metody:**
  - `check_regex_email(cls, email: str)`
  - `check_regex_phone(cls, phone: str)`
  - `check_regex_date(cls, date: str)`
  - `check_regex_words(cls, words: str)`
  - `check_regex_description(cls, description: str)`
  - `check_regex_type_of_job(cls, type_of_job: str)`
  - `check_regex_street(cls, street: str)`

### Database
- třída obsahuje metody pro práci s databází
- **Metody:**
  - `set_up_connection(self)`
  - `execute(self, query)`
  - `execute_with_return(self, query)`
  - `restart(self)`
  - `drop_db(self)`
  - `create_db(self)`
  - `load_view(self, name_of_view: str)`
  - `print_view(self, name_of_view: str)`
  - `format_select(cls, data: list, response: str)`
  - `check_employer_transaction(self, employer_data: str)`
  - `check_customer_transaction(self, customer_data: str)`
  - `transaction(self, employer_data: str, customer_data: str)`

##MVC
### Model
- **Metody:**
  - `restart(self)`
  - `import_data_from_csv(self)`
  - `contract_details_view_report(self)`
  - `job_summary_view_report(self)`
  - `transaction(self, data1: str, data2: str)`
  - `db_select(self, dao, attr: list, end: str)`
  - `db_insert(cls, dao, db_object)`
  - `db_update(cls, dao, value: str, set_to: str)`
  - `db_delete(cls, dao, value: str)`
### View
- **Metody:**
  - `reset(self)`
  - `print_line(self, symbol="=")`
  - `print_message(self)`
  - `print_report(self)`
  - `table_menu_input(self)`
  - `crud_menu_input(self)`
  - `main_menu_input(self)`
  - `transaction_input(self)`
  - `crud_input(self)`
  - `print_select_db_crud(self)`
  - `print_insert_db_crud(self)`
  - `print_update_db_crud(self)`
  - `print_delete_db_crud(self)`
  - `import_db_data(self)`
  - `confirm_remove(cls, question: str)`
  - `update(self)`
### Contreoller
- **Metody:**
  - `run(self)`
  - `terminate(self)`
  - `show_crud_input(self, crud_table: str)`
  - `show_table_menu_input(self, crud_command: str)`
  - `print_report(self)`
  - `show_crud_menu_input(self)`
  - `exe_select_crud(self)`
  - `exe_insert_crud(self)`
  - `exe_update_crud(self)`
  - `exe_delete_crud(self)`
  - `show_transaction_input(self)`
  - `do_transaction(self)`
  - `import_data(self)`
  - `restart_db(self)`

![UML diagram](https://github.com/JirkS/PV-database-Alfa/blob/main/UMLClassDiagram.drawio.png)

## Konfigurační soubor
- Všechno potřebné je nastaveno v souboru config.ini, který lze najít ve složce config/config.ini.
- Pro uživatele jsou k dispozici části "USER-STATIC" - základní nastavení.
### "[DB]":
- driver = {ODBC Driver 17 for SQL Server}
- server = ip adresa nebo jmeoo serveru (193.85.203.188)
- database = jmeno databáze (syrovatko)
- uid = prihlasovaci jmeno (syrovatko)
- pwd = heslo
- VŠE JE NASTAVENO DEFAULTNĚ V config.ini PRO SERVER => NENÍ POTŘEBA NIC MĚNIT (včetně hesla)
### "[USER-STATIC]":
- csv_import_data_file = zadejte cestu k .csv souboru, který chcete importovat. Defaultně je cesta nastavená na složku input/nazev_souboru.csv

## Chybové stavy
- Chyby můžou nasta s požadavky na databázi, která je špatně nekonfigurovaná v config.ini, nebo dotaz na databázi je neproveditelný
- program hlídá chybové stavy: Exception, ValueError, IndexError, FileNotFoundError, PermissionError

## Knihovny třetích stran
- Program využívá jedné knihovny ve třídě database, která ji používá pro připojení k databázi a přáci s ní: import pyodbc

## Závěrečné resumé
- Tento program byl vyvíjen v rámci domácí úlohy Alfa 3  Databázový systém pro povinný maturitní předmět Programové Vybavení.
- Technologie, které program využívá jsou vyučovány ve škole v povinném maturitním předmětu PV - programové vybavení a volitelném maturitním předmětu DS - databázové systémy
- Projekt byl vyvíjen něco málo přes 50 hodin.


