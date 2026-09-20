# TECHNICAL_NOTES
## Πιλοτικό Σύστημα Γεωχωρικών Δεδομένων — Διεύθυνση Δασών Ευβοίας

**Κατάσταση:** Frozen prototype  
**Έκδοση front-end:** v7.2  
**Ημερομηνία τεχνικής αποτύπωσης:** 2026-09-20

Το παρόν αρχείο περιγράφει την τεχνική κατάσταση της πιλοτικής λύσης, τις βασικές αποφάσεις που έχουν ληφθεί και τις αλλαγές που έχουν ήδη εφαρμοστεί.

## 1. Βασική αρχιτεκτονική

```text
Enterprise Geodatabase / SDE
        ↓
Hosted Feature Layer (source)
        ↓
4 Hosted Feature Layer Views
        ↓
4 Web Maps
        ↓
Custom GitHub Pages front-end
```

Το Hosted Feature Layer αποτελεί ανεξάρτητη δημοσιευμένη δομή. Οι αλλαγές στο Hosted περιβάλλον **δεν συγχρονίζονται αυτόματα πίσω στην SDE**.

## 2. Κεντρική SDE

### 2.1 Connection

```text
C:\Users\User\AppData\Roaming\Esri\ArcGISPro\Favorites\DdeSpatial2026_sde.sde
```

### 2.2 Versioning

Τα βασικά Feature Classes είναι:

- registered as versioned,
- Traditional Versioning,
- συνδεδεμένα μέσω `sde.DEFAULT`.

Δεν δημιουργήθηκε child version για το normalization του `DASARXEIO`.

### 2.3 GlobalIDs

Όλα τα βασικά Feature Classes έχουν GlobalID.

### 2.4 Editor Tracking

Όλα τα βασικά Feature Classes έχουν Editor Tracking με:

- `created_user`
- `created_date`
- `last_edited_user`
- `last_edited_date`

## 3. Feature Classes

Κύρια επιχειρησιακά Feature Classes:

1. `KERAIES`
2. `PRAXEIS_2B`
3. `Dasologio`
4. `KTINOTROFIKA`
5. `AN_2026`
6. `AR_AN_2026`
7. `Monopatia_Evoias`
8. `Egkriseis_epemvasis_3_2`

Βοηθητικό / διοικητικό Feature Class:

9. `FOREST_SERVICES_ORIGINAL`

Όλα χρησιμοποιούν EPSG:2100 / Greek Grid.

## 4. FOREST_SERVICES_ORIGINAL

Χρησιμοποιείται ως reference dataset για τα Δασαρχεία.

Canonical τιμές:

- `Αλιβερίου`
- `Ιστιαίας`
- `Λίμνης`
- `Χαλκίδας`

Κατανομή:

- Αλιβερίου: 109
- Ιστιαίας: 8
- Λίμνης: 19
- Χαλκίδας: 13

Σύνολο: 149 polygons.

## 5. Κανονικοποίηση DASARXEIO στην SDE

Στις 2026-09-20 ολοκληρώθηκε normalization του πεδίου `DASARXEIO`.

### 5.1 Scope

Έγιναν μόνο:

- δημιουργία `DASARXEIO` όπου έλειπε,
- μεταφορά από `DASARCHEIO`,
- μεταφορά από `DASARXEIA`,
- κανονικοποίηση τιμών στις 4 canonical τιμές,
- διαγραφή των παλιών `DASARCHEIO` / `DASARXEIA`.

Δεν έγιναν:

- spatial reassignment,
- split γραμμών,
- split πολυγώνων,
- αλλαγές γεωμετρίας,
- `SOURCE_GID`,
- νέο version,
- reconcile/post.

### 5.2 Αποτελέσματα

#### KERAIES
- 99 records
- Αλιβερίου: 53
- Ιστιαίας: 14
- Λίμνης: 12
- Χαλκίδας: 20

#### PRAXEIS_2B
- 118 records
- Αλιβερίου: 111
- Ιστιαίας: 7

#### Dasologio
- 375 records
- Αλιβερίου: 168
- Ιστιαίας: 34
- Λίμνης: 106
- Χαλκίδας: 67

#### KTINOTROFIKA
- 55 records
- δημιουργήθηκε `DASARXEIO`
- μεταφέρθηκαν 55 τιμές από `DASARCHEIO`
- διαγράφηκε `DASARCHEIO`
- Αλιβερίου: 30
- Ιστιαίας: 1
- Λίμνης: 8
- Χαλκίδας: 16

#### AN_2026
- 951 records
- Αλιβερίου: 557
- Ιστιαίας: 44
- Λίμνης: 9
- Χαλκίδας: 340
- NULL: 1

#### AR_AN_2026
- 2402 records
- Αλιβερίου: 843
- Ιστιαίας: 718
- Λίμνης: 40
- Χαλκίδας: 537
- NULL: 264

#### Monopatia_Evoias
- 153 records
- δημιουργήθηκε `DASARXEIO`
- NULL: 153

Δεν έγινε spatial assignment ούτε split.

#### Egkriseis_epemvasis_3_2
- 182 records
- δημιουργήθηκε `DASARXEIO`
- μεταφέρθηκαν 182 τιμές από `DASARXEIA`
- διαγράφηκε `DASARXEIA`
- Αλιβερίου: 49
- Ιστιαίας: 48
- Λίμνης: 65
- Χαλκίδας: 20

## 6. Backup πριν το normalization

Validated backup:

```text
C:\DD_Evoias_Backups\DD_Evoias_PRE_NORMALIZATION_20260920_110409.gdb
```

Manifest:

```text
C:\DD_Evoias_Backups\DD_Evoias_PRE_NORMALIZATION_20260920_110409_manifest.json
```

Επιβεβαιώθηκαν:

- ίδια row counts,
- ίδια GlobalID counts,
- 0 duplicate GlobalIDs,
- geometry QA,
- overall status `OK`.

## 7. Hosted Feature Layer — Source

Service:

```text
DdeSpatial2026_20260913_2308_WFL
```

Item ID:

```text
8202ef3d5f594b5a8aced95d42b001ef
```

REST service:

```text
https://services6.arcgis.com/GAyN9WJ2dkh4VWJ1/arcgis/rest/services/DdeSpatial2026_20260913_2308_WFL/FeatureServer
```

Layer IDs:

| ID | Layer |
|---:|---|
| 0 | KERAIES |
| 1 | PRAXEIS_2B |
| 2 | Dasologio |
| 3 | KTINOTROFIKA |
| 4 | AN_2026 |
| 5 | AR_AN_2026 |
| 6 | Monopatia_Evoias |
| 10 | Egkriseis_epemvasis_3_2 |

Τρέχουσες source settings:

- private / owner-only,
- deletion protection ON,
- public data collection OFF,
- editing OFF στο source,
- editor tracking ενεργό,
- sync ON,
- change log OFF,
- export ON.

## 8. Hosted Feature Layer Views

- Ιστιαίας: `f0fb034c96ac4d04a4131ad16c988e39`
- Λίμνης: `f4914f571b9d4d3bb24dc6b83aabb1aa`
- Χαλκίδας: `56a8991c684d4bf986d2965e19b61bb8`
- Αλιβερίου: `0ec4bc2717054e739cb81a2080856d4e`

Κάθε View περιλαμβάνει layer IDs:

```text
0, 1, 2, 3, 4, 5, 6, 10
```

Κάθε View φιλτράρει το `DASARXEIO` με τη σχετική canonical τιμή.

View settings prototype:

- public,
- deletion protection ON,
- public data collection ON,
- editing ON,
- Add/Delete/Update ON,
- attributes + geometry,
- editors see all,
- editors edit all,
- anonymous editors same as signed-in,
- sync OFF,
- export OFF.

## 9. Web Maps

- Ιστιαίας: `0d593f53db9b4334adc69a40694d10db`
- Λίμνης: `89dd24123e794620a46aad672ae89b58`
- Χαλκίδας: `b8c2c95807a74cf688cfdce364d732f1`
- Αλιβερίου: `0f4a6c8d12924d16b7255554e4eef9f9`

Κάθε Web Map συνδέεται με το αντίστοιχο View.

Έχουν ενημερωθεί:

- Description,
- Terms of Use,
- extent,
- sharing,
- deletion protection,
- περιττές επιλογές όπου κρίθηκε αναγκαίο.

## 10. Front-end

Repository:

```text
https://github.com/asapanidis/dd-evoias-gis
```

Live site:

```text
https://asapanidis.github.io/dd-evoias-gis/
```

Stable έκδοση:

```text
v7.2
```

### 10.1 Λειτουργίες

- office switching,
- Layer List,
- Attribute Table,
- permanent GIS upload,
- Shapefile templates,
- GeoJSON upload,
- field mapping review,
- temporary GIS display,
- Sketch tools,
- full editing,
- create/update/delete,
- zoom to selected feature,
- forced `DASARXEIO` από active office κατά το upload.

### 10.2 Attribute Table

- read-only,
- επιλογή layer,
- row selection,
- zoom-to-feature,
- απόκρυψη GlobalID / SOURCE_GID / system geometry fields.

### 10.3 Upload

Η μόνιμη εισαγωγή:

- υποστηρίζει ZIP Shapefile / GeoJSON,
- exact field mapping πρώτα,
- fallback `LEFT(TargetField,10)` για DBF field names,
- μπλοκάρει ambiguous mappings,
- εμφανίζει review,
- αγνοεί system fields,
- επιβάλλει `DASARXEIO` από το active office,
- προβάλλει σε EPSG:2100 πριν από `applyEdits`.

## 11. Shapefile templates

- `KERAIES_template.zip`
- `PRAXEIS_2B_template.zip`
- `Dasologio_template.zip`
- `KTINOTROFIKA_template.zip`
- `AN_2026_template.zip`
- `AR_AN_2026_template.zip`
- `Monopatia_Evoias_template.zip`
- `Egkriseis_epemvasis_3_2_template.zip`

## 12. Χωρικές επεξεργασίες μόνο στο prototype WFL

### 12.1 Polygon reassignment

Χρησιμοποιήθηκαν:

- `FOREST_SERVICES_ORIGINAL`,
- largest-overlap assignment,
- nearest fallback.

Αρχικός έλεγχος: 4183 polygons.

Αποτελέσματα:

- overlap: 4165
- nearest: 17
- changed: 280
- already correct: 3902
- unresolved: 1

Το unresolved invalid record διαγράφηκε στο prototype.

### 12.2 Monopatia_Evoias

Στο WFL:

- αρχικά 153 lines,
- split στα όρια,
- outside segments με nearest assignment,
- compact by `(SOURCE_GID, DASARXEIO)`,
- τελικό αποτέλεσμα: 170 records.

Κατανομή:

- Ιστιαίας: 26
- Λίμνης: 32
- Χαλκίδας: 60
- Αλιβερίου: 52

Η συγκεκριμένη επεξεργασία **δεν έχει εφαρμοστεί στην SDE**.

## 13. Authentication

Έχει σχεδιαστεί αλλά δεν έχει υλοποιηθεί.

Προτεινόμενο μοντέλο:

- 1 `admin` — όλα τα Δασαρχεία,
- 4 `editor` — ένας ανά Δασαρχείο,
- 1 `viewer` — προβολή και των 4 Δασαρχείων.

| Ρόλος | Προβολή | Attribute Table | Προσωρινά | Upload | Full Editing |
|---|---:|---:|---:|---:|---:|
| admin | ✓ | ✓ | ✓ | ✓ | ✓ |
| editor | ✓ | ✓ | ✓ | ✓ | ✓ |
| viewer | ✓ | ✓ | ✓ | ✕ | ✕ |

Η υλοποίηση έχει παγώσει μέχρι να ολοκληρωθεί η αξιολόγηση του prototype.

## 14. REST / URL exposure

Από το v7 αφαιρέθηκαν τα hard-coded FeatureServer URLs από το `index.html`.

Ο uploader βρίσκει τα target layers μέσω του ενεργού Web Map και του `layerId`.

Επειδή η εφαρμογή είναι static GitHub Pages, τα Web Map item IDs και τα network requests παραμένουν τεχνικά παρατηρήσιμα.

Πραγματική προστασία απαιτεί:

- private AGOL resources,
- backend,
- authentication,
- token management.

## 15. QA

Έχει ολοκληρωθεί QA και στα 4 Δασαρχεία για:

- Web Map switching,
- extents,
- Attribute Table,
- permanent upload,
- polygon / polyline upload,
- field mapping,
- `DASARXEIO`,
- create/update/delete,
- geometry editing,
- temporary display,
- temporary sketch,
- refresh / persistence,
- isolation ανά Δασαρχείο,
- λειτουργία με private source + public Views.

Το prototype θεωρείται stable για δοκιμές από τη ΔΔΕ.

## 16. Ανοικτά τεχνικά θέματα

- custom authentication,
- production hosting architecture,
- SDE ↔ Google Drive ↔ Hosted sync,
- spatial reassignment στην SDE εφόσον ζητηθεί,
- νέο publish WFL από normalized SDE,
- διαδικασία εισαγωγής νέου Feature Class,
- migration σε AGOL / Portal ΥΠΕΝ,
- πλήρης handover τεκμηρίωση.
