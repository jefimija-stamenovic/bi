# Analiza prodajnih performansi i ponašanja kupaca uz pomoć Power BI-a

Power BI izveštaj izrađen nad **Olist Brazilian E-Commerce** skupom podataka (javno dostupan na Kaggle), koji pokriva poslovanje brazilske e-commerce platforme Olist u periodu od septembra 2016. do avgusta 2018. godine.

## O projektu

Projekat obuhvata kompletan proces poslovne inteligencije — od uvoza i čišćenja sirovih podataka, preko izgradnje relacionog modela, do kreiranja DAX mera i interaktivnih izveštaja koji analiziraju prodaju, kupce, logistiku i učinak prodavaca.

## Skup podataka

Izvorni skup podataka sastoji se od 9 međusobno povezanih CSV tabela:

| Tabela | Opis |
|---|---|
| `orders` | Centralna tabela narudžbina (status, datumi kupovine i isporuke) |
| `customers` | Podaci o kupcima i njihovoj lokaciji |
| `order_items` | Pojedinačne stavke (proizvodi) unutar narudžbina |
| `order_payments` | Načini i iznosi plaćanja |
| `order_reviews` | Recenzije i ocene kupaca |
| `products` | Šifarnik proizvoda i njihove kategorije |
| `sellers` | Šifarnik prodavaca i njihova lokacija |
| `geolocation` | Geografske koordinate po poštanskom prefiksu |
| `product_category_name_translation` | Prevod naziva kategorija sa portugalskog na engleski |

## Priprema podataka (ETL)

- Uvoz svih tabela putem Power Query (**Get Data → Text/CSV**)
- Provera i ispravka tipova podataka (datumi, decimalni brojevi)
- Rešavanje problema dupliranih zapisa u `geolocation` (agregacija po poštanskom prefiksu putem **Group By**)
- Kreiranje kolone `StateFullName` (pun naziv savezne države + zemlja), neophodne za ispravno geografsko prikazivanje brazilskih regiona na Power BI mapama
- Kreiranje izračunate kolone `DeliveryDays` za analizu vremena isporuke

## Model podataka

Model je organizovan po principu šeme pahuljice (**star schema**), sa tabelom `orders` u centralnoj poziciji. Radi omogućavanja filtriranja u oba smera (npr. filtriranje po prodavcu ili kategoriji proizvoda, koje treba da utiče i na povezane tabele višeg nivoa), sledeće relacije su podešene kao dvosmerne (**Both**):
- `orders` ↔ `order_items`
- `orders` ↔ `order_reviews`
- `orders` i `order_payments`

## Ključne DAX mere

Mere su organizovane u izdvojenoj tabeli `_Measures`, grupisane po analitičkim celinama:

- **Prodaja:** `Total Revenue`, `Total Orders`, `Average Order Value`, `Revenue Growth %`
- **Kupci:** `Total Customers`, `Average Review Score`, `Repeat Customers %`, `Negative Reviews %`
- **Isporuka:** `Average Delivery Time (days)`, `Late Orders %`, `Average Delay (days)`
- **Prodavci:** `Active Sellers`, `Average Seller Rating`, `Average Revenue per Active Seller`, `Cumulative Sellers`

## Izveštaji

| Stranica | Sadržaj |
|---|---|
| **Sales Overview** | Prihod, trendovi kroz vreme, prodaja po kategorijama proizvoda |
| **Customer Overview** | Geografska raspodela, segmentacija i zadovoljstvo kupaca |
| **Delivery Overview** | Operativna efikasnost isporuke, kašnjenja po regionu |
| **Seller Overview** | Prihod, ocene i pouzdanost prodavaca |

Svaka stranica sadrži segmentatore (godina, savezna država, kategorija proizvoda) koji omogućavaju detaljnije istraživanje podataka.

## Alati

- Power BI Desktop (Power Query, DAX, modelovanje podataka)
- Izvor podataka: [Olist Brazilian E-Commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

Projekat je izrađen u okviru seminarskog rada na temu Power BI i poslovne inteligencije.
