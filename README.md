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
- 🎲 **Fleet generator** — Create random test fleets with configurable weight profiles (Normal 65–95 kg, Disperso 55–115 kg, Concentrated, Bimodal) and gender-consistent names
- 📊 **Three-way comparison** — Fixed vs Split vs Elastic side by side
- 🏅 **Age classes** — Automatic classification (Junior, Youth, Open, Master, G.Master, Legend, S.Legend)
- ⚥ **Gender separation** — Optionally divide males and females into independent groups
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

## Parameters

### Maximum weight range per group (default: 8 kg)

Maximum weight difference (in kg) allowed within a single group. For example, with 8 kg, if the lightest sailor in a group weighs 72 kg, the heaviest cannot exceed 80 kg.

Slider range: **2–16 kg** (at 16 kg the limit is removed — any range is accepted).

- **Lower values** (2–6 kg) → more homogeneous groups, but may produce unequal group sizes
- **Higher values** (10–15 kg) → more flexibility for balanced sizes, but wider weight spread within groups
- **16+ kg** → no range limit applied

### Maximum group size (default: 33%)

Slider range: **20–50%**. Maximum percentage of the total fleet that any single group can contain. For example, with 33% and 30 sailors, no group will exceed 10 competitors.

- **Lower values** (20–30%) → forces very even group sizes
- **Higher values** (40–50%) → allows more flexibility, useful when weight distribution is very uneven

### Number of groups (default: 4)

Number of groups to divide the fleet into. Range: **2 to 8**.

With 4 groups the **Fixed** system is also enabled (IWCA cut-offs: 70 / 79 / 88 kg). With any other value, Fixed is disabled.

### Age factor (default: 0)

Controls how much priority the algorithm gives to keeping competitors of the same age category together. Scale from 0 to 10.

The algorithm scores each possible partition using a cost function:

```
cost = α × sizeBalance + β × weightRange + γ × agePenalty
```

Where `γ = ageFactor / 10`. The **agePenalty** measures how many age categories (Junior, Youth, Master, G.Master, Legend, S.Legend) are fragmented across multiple groups. The **Open** category (ages 20–39) is always excluded from this calculation as it is typically the largest.

| Value | Effect |
|-------|--------|
| **0** | Age is ignored — optimises only for weight and group size |
| **1–4** | Mild preference — tries to keep age categories together without sacrificing much weight balance |
| **5–7** | Balanced — meaningful trade-off between weight homogeneity and age grouping |
| **8–10** | Strong priority — will accept slightly worse weight distribution to keep juniors, masters, etc. together |

**Age categories:**

| Category | Age range |
|----------|-----------|
| Junior | Under 15 |
| Youth | 15–19 |
| Open | 20–39 |
| Master | 40–49 |
| G.Master | 50–59 |
| Legend | 60–69 |
| S.Legend | 70+ |

## Algorithm

1. Sort competitors by weight (ties broken by age per IWCA Rule H.3.3.2.ii)
2. Evaluate all possible cut partitions into N consecutive groups (2–8)
3. Filter by hard constraints (max range, max size)
4. Score valid partitions by: size balance + range minimisation + age cohesion
5. If no valid partition exists, progressively relax constraints and report

Complexity: O(N^(G-1)) where G is the number of groups — instant for typical fleets up to 200 competitors.

## Proposed by

- **Borja Hernández Medina** — Original idea
- **Pedro Prats Hernández** — Development

## Reference

IWCA Class Rules, Section H.3 — Weight / Age Divisions

## License

MIT
