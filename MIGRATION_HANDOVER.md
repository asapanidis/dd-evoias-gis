# MIGRATION_HANDOVER
## Πιλοτικό Σύστημα Γεωχωρικών Δεδομένων — Διεύθυνση Δασών Ευβοίας

**Σκοπός:** Καθοδήγηση για μελλοντική μεταφορά της πιλοτικής λύσης σε παραγωγική ή άλλη εγκεκριμένη υποδομή.

## 1. Βασική αρχή

Η τρέχουσα λύση είναι πιλοτική.

Η παραγωγική υλοποίηση δεν θα πρέπει να θεωρηθεί απλή «αντιγραφή» του υπάρχοντος prototype. Πριν από οποιαδήποτε μετάβαση απαιτείται απόφαση της Διεύθυνσης Δασών Ευβοίας σχετικά με:

- ποια υποδομή θα είναι η επίσημη,
- ποια βάση θα θεωρείται master,
- ποιοι χρήστες θα έχουν δικαιώματα,
- αν θα εφαρμοστεί χωρική επαναταξινόμηση ανά Δασαρχείο,
- αν απαιτείται custom authentication,
- πώς θα γίνεται η μελλοντική συντήρηση και ο συγχρονισμός.

## 2. Πιθανά σενάρια μετάβασης

### Σενάριο Α — Υποδομή ΥΠΕΝ

Μεταφορά σε:

- ArcGIS Online του ΥΠΕΝ,
- ArcGIS Enterprise / Portal,
- ή άλλη επίσημη υποδομή του φορέα.

Πλεονεκτήματα:

- επίσημη ιδιοκτησία δεδομένων και εφαρμογής,
- κεντρική διαχείριση χρηστών,
- θεσμικός έλεγχος,
- ευκολότερη ένταξη σε ευρύτερη υποδομή ΥΠΕΝ.

### Σενάριο Β — Νέο ArcGIS Online περιβάλλον

Δημιουργία ξεχωριστού ArcGIS Online organization για την παραγωγική λύση.

Πλεονεκτήματα:

- καθαρός διαχωρισμός από το πιλοτικό περιβάλλον,
- ελεγχόμενη μεταφορά items,
- δυνατότητα νέας πολιτικής χρηστών και groups.

### Σενάριο Γ — Νέα σύμβαση υποστήριξης / φιλοξενίας

Η πιλοτική εφαρμογή εξελίσσεται σε παραγωγική υπηρεσία μέσω νέας συμφωνίας.

Πιθανές υπηρεσίες:

- hosting,
- migration,
- authentication,
- monitoring,
- sync,
- QA,
- support,
- προσθήκη νέων layers / workflows.

## 3. Τι πρέπει να μεταφερθεί

### 3.1 Enterprise Geodatabase

- SDE schema,
- Feature Classes,
- GlobalIDs,
- Editor Tracking,
- Traditional Versioning,
- `DASARXEIO`,
- domains / subtypes εφόσον προστεθούν,
- metadata.

### 3.2 Hosted Feature Layer

Νέο publish από την εγκεκριμένη πηγή.

Προτείνεται να μην θεωρηθεί ο σημερινός Hosted source ως οριστικός production source.

### 3.3 Views

Νέα Views ανά Δασαρχείο:

- Ιστιαίας
- Λίμνης
- Χαλκίδας
- Αλιβερίου

Κάθε View να περιλαμβάνει τα απαιτούμενα layers και filter στο canonical `DASARXEIO`.

### 3.4 Web Maps

Νέα Web Maps που συνδέονται με τα νέα Views.

### 3.5 Front-end

Το front-end πρέπει να ενημερωθεί με τα νέα Web Map item IDs.

## 4. Προτεινόμενη ακολουθία migration

### Βήμα 1 — Freeze υπάρχοντος prototype

Καταγράφονται:

- stable έκδοση,
- Web Map IDs,
- View IDs,
- Hosted source,
- GitHub commit / release,
- τελικό QA status.

### Βήμα 2 — Επιβεβαίωση master database

Απαιτείται ρητή απόφαση για το ποια βάση είναι η επίσημη master.

Προτεινόμενη λογική:

```text
SDE = authoritative master
Hosted = δημοσιευμένο / operational web αντίγραφο
```

Αυτό πρέπει να επιβεβαιωθεί θεσμικά.

### Βήμα 3 — Έλεγχος schema

Να ελεγχθούν:

- `DASARXEIO`
- GlobalIDs
- Editor Tracking
- field names
- aliases
- data types
- lengths
- nullability
- geometry type
- EPSG:2100

### Βήμα 4 — Απόφαση για spatial normalization

Η ΔΔΕ πρέπει να αποφασίσει αν θα εφαρμοστεί στην SDE:

- spatial reassignment polygons,
- split στα όρια των Δασαρχείων,
- spatial assignment των μονοπατιών,
- lineage μέσω `SOURCE_GID`.

Σήμερα αυτά έχουν δοκιμαστεί στο WFL αλλά όχι στην SDE.

### Βήμα 5 — Νέο publish

Να δημοσιευτεί νέο Hosted Feature Layer από την εγκεκριμένη normalized SDE.

### Βήμα 6 — Νέα Views

Να δημιουργηθούν 4 νέα Hosted Feature Layer Views.

### Βήμα 7 — Νέα Web Maps

Να δημιουργηθούν 4 νέα Web Maps.

### Βήμα 8 — Front-end configuration

Να αντικατασταθούν τα παλιά item IDs με τα νέα.

### Βήμα 9 — Authentication

Εφόσον απαιτείται production security:

- private resources,
- custom login ή επίσημο identity provider,
- role-based access,
- server-side secrets,
- token management.

### Βήμα 10 — QA

Πλήρης έλεγχος:

- κάθε Δασαρχείου,
- κάθε layer,
- upload,
- editing,
- create/delete,
- temporary tools,
- Attribute Table,
- role restrictions,
- persistence,
- geometry/CRS.

## 5. Πιθανό μοντέλο χρηστών

### Admin
- 1 χρήστης
- πρόσβαση και στα 4 Δασαρχεία
- πλήρης λειτουργικότητα

### Editors
- 4 χρήστες
- 1 ανά Δασαρχείο
- πρόσβαση μόνο στο αντίστοιχο Δασαρχείο
- upload + full editing

### Viewer
- 1 χρήστης
- πρόσβαση και στα 4 Δασαρχεία
- χωρίς permanent upload / editing

## 6. Front-end migration checklist

- [ ] νέο repository ή transfer υπάρχοντος repository
- [ ] νέα Web Map item IDs
- [ ] έλεγχος όλων των templates
- [ ] έλεγχος uploader
- [ ] έλεγχος layer IDs
- [ ] έλεγχος Attribute Table
- [ ] έλεγχος Editor
- [ ] έλεγχος προσωρινών εργαλείων
- [ ] νέα branding στοιχεία εφόσον απαιτούνται
- [ ] νέα URL / domain εφόσον απαιτείται
- [ ] νέα Terms of Use
- [ ] νέο Description
- [ ] production security review

## 7. AGOL / Portal migration checklist

Για κάθε νέο Hosted Layer / View / Map:

- [ ] owner / organization
- [ ] sharing
- [ ] deletion protection
- [ ] editing settings
- [ ] public data collection
- [ ] sync
- [ ] export
- [ ] editor tracking
- [ ] capabilities
- [ ] Description
- [ ] Terms of Use
- [ ] extent
- [ ] layer order
- [ ] symbology
- [ ] Web Map association

## 8. Νέο Feature Class — γενική διαδικασία

Για κάθε νέο FC, π.χ. `ΑΙΟΛΙΚΑ_ΠΑΡΚΑ`, απαιτείται:

1. δημιουργία / εισαγωγή στην SDE,
2. EPSG:2100,
3. GlobalID,
4. Editor Tracking,
5. Traditional Versioning εφόσον απαιτείται,
6. `DASARXEIO`,
7. schema QA,
8. publish / republish,
9. προσθήκη στα Views,
10. προσθήκη στα Web Maps,
11. δημιουργία template,
12. ενημέρωση uploader,
13. ενημέρωση Editor,
14. ενημέρωση Attribute Table,
15. πλήρες QA.

Η ακριβής διαδικασία θα τεκμηριωθεί όταν προστεθεί το πρώτο νέο FC.

## 9. SDE ↔ Hosted synchronization

Δεν έχει ακόμη οριστικοποιηθεί.

Πριν υλοποιηθεί πρέπει να αποφασιστούν:

- master source,
- κατεύθυνση sync,
- συχνότητα,
- conflict policy,
- handling νέων GlobalIDs,
- handling deletes,
- handling schema changes,
- audit / logs,
- rollback.

Δεν συνιστάται ad-hoc αυτόματος συγχρονισμός χωρίς σαφή authoritative source.

## 10. Google Drive

Το Google Drive χρησιμοποιείται στο πλαίσιο της λογικής του «child copy» / ανταλλαγής δεδομένων.

Σε παραγωγική αρχιτεκτονική πρέπει να αποφασιστεί αν:

- θα παραμείνει ως μηχανισμός ανταλλαγής,
- θα αντικατασταθεί,
- θα λειτουργεί μόνο για exports / snapshots,
- θα συμμετέχει σε ημι-αυτόματο sync.

Η τελική σχεδίαση παραμένει ανοικτή.

## 11. Restore / rollback

Υπάρχει validated pre-normalization backup:

```text
C:\DD_Evoias_Backups\DD_Evoias_PRE_NORMALIZATION_20260920_110409.gdb
```

Manifest:

```text
C:\DD_Evoias_Backups\DD_Evoias_PRE_NORMALIZATION_20260920_110409_manifest.json
```

Σε μελλοντικές σημαντικές schema/data αλλαγές πρέπει να δημιουργείται νέο αντίστοιχο restore point.

## 12. Known differences: SDE vs current prototype WFL

Η SDE και το υπάρχον prototype WFL **δεν είναι πλέον απολύτως ταυτόσημα**.

Το prototype WFL έχει υποστεί:

- spatial polygon reassignment,
- διαγραφή ενός invalid polygon,
- split των `Monopatia_Evoias`,
- spatial assignment των μονοπατιών,
- `SOURCE_GID` lineage.

Η SDE έχει υποστεί:

- GlobalIDs,
- Editor Tracking,
- Traditional Versioning,
- βασικό normalization `DASARXEIO`,
- διαγραφή του null-geometry record.

Η ΔΔΕ πρέπει να αποφασίσει αν οι spatial αλλαγές του prototype θα μεταφερθούν στην SDE πριν από νέο production publish.

## 13. Προτεινόμενο νέο publish

Αν αποφασιστεί νέο publish από την normalized SDE:

1. διατηρείται το σημερινό WFL ως frozen prototype,
2. δημοσιεύεται νέο source WFL,
3. δημιουργούνται νέα Views,
4. δημιουργούνται νέα Web Maps,
5. δημιουργείται νέα configuration έκδοση του front-end,
6. γίνεται πλήρες QA,
7. μόνο μετά αποφασίζεται η απόσυρση του prototype.

Δεν προτείνεται αντικατάσταση του υπάρχοντος WFL πριν ολοκληρωθεί το QA του νέου περιβάλλοντος.

## 14. Τελική παράδοση

Μία ολοκληρωμένη handover package θα πρέπει να περιλαμβάνει:

- SDE schema inventory,
- data model,
- field dictionary,
- CRS documentation,
- Web Layer inventory,
- View filters,
- Web Map inventory,
- front-end source code,
- templates,
- README,
- Technical Notes,
- Migration / Handover Notes,
- backup / restore instructions,
- QA checklist,
- known issues / pending decisions.

## 15. Κατάσταση κατά την παρούσα τεχνική αποτύπωση

Το prototype v7.2 είναι frozen και διαθέσιμο για δοκιμές από τη ΔΔΕ.

Η επόμενη τεχνική κατεύθυνση θα καθοριστεί από:

- παρατηρήσεις της ΔΔΕ,
- πιθανές νέες απαιτήσεις,
- απόφαση για spatial normalization στην SDE,
- απόφαση για νέο publish,
- απόφαση για production hosting και authentication.
