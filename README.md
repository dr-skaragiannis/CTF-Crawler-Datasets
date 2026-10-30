# Οδηγίες χρήσης — CTF-Crawler

Σύντομος οδηγός για το στήσιμο και τη χρήση των δύο εργαλείων σε ένα CTF εργαστήριο
(δικά σου ή εγκεκριμένα μηχανήματα).

## 1. Λήψη του κώδικα

Κατέβασε το repository και μπες στον φάκελό του:

```bash
git clone https://github.com/dr-skaragiannis/CTF-Crawler.git
```

```bash
cd CTF-Crawler
```

```bash
cat README.md
```

## 2. Εγκατάσταση στο Kali (επιτιθέμενος)

```bash
sudo apt install -y cargo tcpdump bsdutils openssh-client
```

```bash
cargo build --release
```

```bash
sudo install -m 755 target/release/ctf-logger target/release/ctf-victim-logs /usr/local/bin/
```

## 3. Καταγραφή από τον επιτιθέμενο — `ctf-logger`

Γίνε root και ξεκίνα την καταγραφή:

```bash
sudo su
```

```bash
ctf-logger
```

Το πρόγραμμα θα ζητήσει το **όνομα του challenge** και την **IP του στόχου**, και μετά
καταγράφει ζωντανά (pcap όλων των interfaces, εντολές και έξοδο κάθε terminal, κλειδιά
TLS και ιστορικό browser, κίνηση του Burp) μέχρι να πατήσεις **Ctrl+C**. Τα αρχεία
αποθηκεύονται στο `/home/<χρήστης>/CTF-Logs/<challenge>/<ημερομηνία_ώρα>/`.

## 4. Προετοιμασία του θύματος - **Αφού αποκτήσεις πρόσβαση root** (victim VM)

**Αφού αποκτήσεις πρόσβαση root** στο μηχάνημα-θύμα, δημιούργησε έναν νέο χρήστη και δώσε
του δικαιώματα sudo. Έτσι το `ctf-victim-logs` συνδέεται με αυτόν τον χρήστη αντί για
τον λογαριασμό root.

Δημιούργησε τον χρήστη (τρέχοντας ως root στο θύμα):

```bash
useradd -m <username>
```

Όρισε κωδικό που θα θυμάσαι (π.χ. ίδιο με το username):

```bash
passwd <username>
```

Δώσε στον χρήστη δικαιώματα sudo (τον βάζεις στην ομάδα `sudo`):

```bash
usermod -aG sudo <username>
```

## 5. Συλλογή logs από το θύμα — `ctf-victim-logs`

Στη συνέχεια, από το **Kali**, τρέξε την παρακάτω εντολή. Ανοίγει σύνδεση SSH και,
χρησιμοποιώντας τον νέο χρήστη, κατεβάζει όλα τα logs από το victim VM:

```bash
ctf-victim-logs
```

Θα ζητήσει το challenge, την IP του θύματος, τον χρήστη SSH (βάλε το `<username>` που
έφτιαξες), τη θύρα και (αν χρειαστεί) τον κωδικό sudo. Τα αρχεία κατεβαίνουν στο
`CTF-Logs/<challenge>/victim_<ip>_<ημερομηνία_ώρα>/`, όπου οι συλλογές `.tar.gz`
αποσυμπιέζονται αυτόματα στον φάκελο `extracted/`.

> **Σημείωση:** δημιούργησε νέο χρήστη μόνο σε μηχανήματα που σου ανήκουν ή που έχεις
> άδεια να δοκιμάσεις (π.χ. εργαστήριο CTF). Για περισσότερες λεπτομέρειες δες το
> [README.md](README.md).
>


## Fork του Αποθετηρίου

### Επιλογή Α: Μέσω της Ιστοσελίδας του GitHub

1. Επισκεφθείτε τη σελίδα του έργου: [dr-skaragiannis/CTF-Crawler-Datasets](https://github.com/dr-skaragiannis/CTF-Crawler-Datasets).
2. Κάντε κλικ στο κουμπί **Fork** (επάνω δεξιά).
3. Επιλέξτε τον λογαριασμό σας και πατήστε **Create fork**.

---

## Κλωνοποίηση (Clone) Τοπικά

Αν κάνατε το fork από την ιστοσελίδα, κατεβάστε το αντίγραφο στον υπολογιστή σας:

```bash
# Αντικαταστήστε το <ΤΟ_GITHUB_USERNAME_ΣΑΣ> με το όνομα χρήστη σας στο GitHub και κατεβάστε το fork σε zip (download) ή μεσω της παρακάτω εντολής
git clone https://github.com/<ΤΟ_GITHUB_USERNAME_ΣΑΣ>/CTF-Crawler-Datasets.git

# Μεταβείτε στον κατάλογο του αποθετηρίου
cd CTF-Crawler-Datasets
```

---

## Δημιουργία Φακέλου και Προσθήκη Δεδομένων

```bash
# 1. Δημιουργία του νέου φακέλου με αριθμό μητρώου σας
mkdir Αριθμός-Μητρώου-σας

# 2. Αντιγραφή των αρχείων/δεδομένων σας μέσα στον φάκελο
# Κάντε copy paste ολόκληρο το φάκελο (πχ. Raven) με τα logs και τα δεδομένα που συλλέξατε με το ctf-logger και το ctf-victim-logs
```

## Stage, Commit και Push

```bash
# Έλεγχος των νέων/τροποποιημένων αρχείων
git status

# Προσθήκη του φακέλου στο staging area
git add .

# Δημιουργία commit με περιγραφικό μήνυμα
git commit -m "Αριθμός Μητρώου και όνομα του Challenge"

# Αποστολή των αλλαγών στο fork σας στο GitHub
git push
```
Θα σας ζητήσει username και κωδικό. Για username θα πάτε στο github πάνω δεξιά, settings και έπειτα στο μενού αριστερά Developer Options.
Έπειτα Personal access tokens -> Tokens Classic - Generate New Token. Έπειτα επιλέγετε τα permissions (επιλέξτε αρκετά, ειδικά αυτά που είναι για push) και έπειτα το token μπορεί να 

Έπειτα θα έχω τη δυνατότητα να δω το fork σας καθώς θα έχετε κάνει pull request (από το Github πράσινο κουμπί pull request). Πάντα τοποθετείτε το ΑΜ και το όνομα του Challenge.
