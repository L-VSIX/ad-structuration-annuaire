# 🗂️ Structuration de l'annuaire Active Directory

> Organisation du domaine raidaporter.local en OU par service métier, avec groupes de sécurité à trois niveaux de droits.

## Organisation du domaine

Le domaine matérialise l'organigramme de RAID-A-PORTER : direction générale + 5 directions opérationnelles (Finance, Commerce, Technique, RH, Logistique) + direction Informatique, pour ~60 comptes utilisateurs.


## [Durcissement appliqué](https://www.it-connect.fr/comment-creer-un-domaine-active-directory-respectueux-des-bonnes-pratiques-de-securite/#google_vignette)

Le déploiement initial de l'AD a été **repris en cours de projet** pour appliquer les bonnes pratiques : compte d'administration nominatif distinct du compte par défaut, mot de passe fort sur le compte de délégation, stratégie de mots de passe robuste.

<img width="1524" height="996" alt="2pingcastle" src="https://github.com/user-attachments/assets/f9a6a178-bc97-4ba4-aa70-dcfd1f499dbe" />

## Structuration des OU

L'OU racine `RAIDAPORTER` regroupe des sous-OU : `Groupes`,`Serveurs`,`Utilisateurs` et `Ordinateurs`.
L'OU `Groupes` rassemble les groupes applicatifs du systèmes d'informations. On y retrouve les sous-OU `Fichier` utilisé pour les groupes d'ayant droit et `Organigramme` utilisé pour structuré les utilisateurs d'après l'organigramme RH.
Chaque service métier possède préfixée numériquement pour le tri, avec 3 groupes de sécurité :

- `_R` (Responsable) — Responsable du service
- `_M` (Membre) — Membre du service
- `_G` (Gestionnaire) — Gestionnaire du service

```powershell
New-ADOrganizationalUnit -Name '110000_SCE_FINANCE' `
  -Path 'OU=Utilisateurs,OU=RAIDAPORTER,DC=raidaporter,DC=local' `
  -ProtectedFromAccidentalDeletion $true
```
<img width="1149" height="655" alt="ad" src="https://github.com/user-attachments/assets/870c07d8-a18f-49fa-8ba5-2ca1c1af9bc6" />

## Incident résolu

Un **renommage à chaud d'un contrôleur de domaine** n'est pas supporté et a nécessité un redéploiement complet du DC concerné — leçon retenue pour la suite du projet.

## Repos liés

- [`ad-automatisation-powershell`](https://github.com/L-VSIX/ad-automatisation-powershell)
- [`dfs-architecture`](https://github.com/L-VSIX/dfs-architecture) — permissions NTFS alignées sur ces groupes
- [`orangehrm-gestion-comptes`](https://github.com/L-VSIX/orangehrm-gestion-comptes) — consommateur de l'annuaire

## Auteur

**Lilian Vertueux** — [LinkedIn](https://www.linkedin.com/in/lilian-vertueux/)
