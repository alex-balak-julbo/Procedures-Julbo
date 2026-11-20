# Procédure d'installation et de connexion à une base Oracle via ODBC sous Windows 11

## 1. Télécharger Oracle Instant Client

Télécharger les packages Oracle Instant Client correspondant à l'architecture du poste (64 bits) :

- Basic Package
  - `instantclient-basic-windows.x64-xx.x.x.x.x.zip`
- ODBC Package
  - `instantclient-odbc-windows.x64-xx.x.x.x.x.zip`

Exemple :

```text
instantclient-basic-windows.x64-23.26.3.0.0.zip
instantclient-odbc-windows.x64-23.26.3.0.0.zip
```

## 2. Installer Instant Client

Créer le dossier :

```text
C:\Oracle\instantclient_23_26
```

Décompresser le contenu des deux ZIP dans ce même dossier.

On doit notamment retrouver :

```text
oci.dll
oraociei23.dll
sqora64.dll
odbc_install.exe
```

## 3. Ajouter Oracle au PATH Windows

Ouvrir :

```text
Paramètres système avancés
→ Variables d'environnement
→ Path
```

Ajouter :

```text
C:\Oracle\instantclient_23_26
```

## 4. Installer le pilote ODBC Oracle

Ouvrir un CMD en administrateur :

```cmd
cd C:\Oracle\instantclient_23_26
odbc_install.exe
```

## 5. Vérifier l'installation

Ouvrir :

```text
C:\Windows\System32\odbcad32.exe
```

Onglet **Pilotes** et vérifier la présence de :

```text
Oracle in instantclient_23_26
```

## 6. Créer un DSN Oracle

Dans `odbcad32.exe` :

- Onglet **DSN Système**
- **Ajouter**
- Sélectionner `Oracle in instantclient_23_26`

## 7. Paramétrer la connexion

Exemple :

| Champ | Valeur |
|---------|---------|
| Data Source Name | WMS PROD |
| Description | WMS PROD |
| TNS Service Name | WMSPROD |
| User ID | WM |

### Important

Le champ :

```text
TNS Service Name
```

Doit contenir le **SERVICE_NAME Oracle** et non le nom du serveur.

✅ Correct :

```text
WMSPROD
```

❌ Incorrect :

```text
srvwms.julbo.fr
```

## 8. Tester la connexion

Cliquer sur :

```text
Test Connection
```

Saisir l'utilisateur et le mot de passe Oracle.

Si tout est correct :

```text
Connection Successful
```

## 9. Utilisation dans Excel

```text
Données
→ Obtenir des données
→ À partir d'autres sources
→ ODBC
```

Choisir :

```text
WMS PROD
```

Exemple de requête :

```sql
SELECT *
FROM WM.ARTICLE;
```

## Dépannage

### ORA-12504

Erreur :

```text
ORA-12504: listener was not given the SERVICE_NAME
```

Cause :

```text
TNS Service Name incorrect ou vide
```

Solution :

Utiliser le véritable SERVICE_NAME Oracle (exemple : `WMSPROD`).

### Architecture incorrecte

- Excel 64 bits → Oracle 64 bits
- Excel 32 bits → Oracle 32 bits

Les architectures doivent être identiques.

## Configuration validée

```text
DSN              : WMS PROD
TNS Service Name : WMSPROD
Utilisateur      : WM
Pilote           : Oracle Instant Client 23.26 (64 bits)
```
