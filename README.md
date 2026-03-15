# Windsurfer — Elastic Weight Division

**A fair algorithm for dividing Windsurfer Class competitors into weight groups.**

## The problem

The Windsurfer Class Rules (Section H.3) provide two methods for dividing competitors into four weight groups:

- **Fixed Weights**: Predetermined cut-offs (70 / 79 / 88 kg). In practice, most sailors weigh between 73–83 kg, causing groups B and C to overflow while A and D are nearly empty.
- **Split Weights**: Equal-sized quartiles. This forces competitors with a 15+ kg difference into the same group — competitively unfair.

## The solution

The **Elastic Weight Division** is a hybrid system that automatically finds the optimal division by respecting two configurable constraints:

1. **Maximum weight range per group** (default: 8 kg)
2. **Maximum group size** (default: 33% of fleet)

An optional **age factor** can keep competitors from the same IWCA age category together.

## Features

- 🌍 **Bilingual** — Spanish / English interface
- 📂 **CSV import** — Load competitor lists from a file
- ✏️ **Manual entry** — Add competitors with name, gender, sail number, nationality, age & weight
- 🏳️ **Country selector** — Dropdown with flags and ISAF sailing codes (ESP, GER, NED…)
- 🎲 **Fleet generator** — Create random test fleets with configurable profiles
- 📊 **Three-way comparison** — Fixed vs Split vs Elastic side by side
- 🏅 **Age classes** — Automatic classification (Junior, Youth, Open, Master, G.Master, Legend, S.Legend)
- 🖨️ **Print / PDF** — Clean printable output with all results
- 📱 **PWA** — Works offline, installable on any device
- 🔒 **No server needed** — Everything runs in the browser

## Usage

### Online
Visit the published site and start using it immediately.

### Offline
1. Download the repository
2. Open `index.html` in any browser
3. It works without internet

### CSV format
```
Name,Gender,Sail,Country,Age,Weight
Carlos Ruiz,M,1234,ESP,42,78.5
Marta Vidal,F,5678,FRA,28,73.0
Hugo Bergström,M,9012,SWE,55,85.0
```
Only **Name** and **Weight** are required. Gender (M/F), Sail number, Country and Age are optional.

### Country codes (ISAF sailing codes)

| Code | Country | Code | Country | Code | Country |
|------|---------|------|---------|------|---------|
| ESP | Spain | FRA | France | ITA | Italy |
| GER | Germany | POR | Portugal | NED | Netherlands |
| GBR | United Kingdom | SWE | Sweden | NOR | Norway |
| DEN | Denmark | FIN | Finland | SUI | Switzerland |
| AUT | Austria | POL | Poland | CZE | Czech Republic |
| HUN | Hungary | CRO | Croatia | GRE | Greece |
| TUR | Turkey | UKR | Ukraine | IRL | Ireland |
| BEL | Belgium | ROU | Romania | SRB | Serbia |
| SLO | Slovenia | SVK | Slovakia | BRA | Brazil |
| ARG | Argentina | USA | United States | JPN | Japan |
| AUS | Australia | NZL | New Zealand | CAN | Canada |
| CHI | Chile | MEX | Mexico | COL | Colombia |
| ISR | Israel | RSA | South Africa | CHN | China |
| KOR | South Korea | THA | Thailand | IND | India |

## Algorithm

1. Sort competitors by weight (ties broken by age per IWCA Rule H.3.3.2.ii)
2. Evaluate all possible 3-cut partitions into 4 consecutive groups
3. Filter by hard constraints (max range, max size)
4. Score valid partitions by: size balance + range minimisation + age cohesion
5. If no valid partition exists, progressively relax constraints and report

Complexity: O(N³) — instant for fleets up to 200 competitors.

## Proposed by

- **Borja Hernández Medina** — Original idea
- **Pedro Prats Hernández** — Development

## Reference

IWCA Class Rules, Section H.3 — Weight / Age Divisions

## License

MIT
