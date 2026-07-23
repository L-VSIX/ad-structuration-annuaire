# 🗂️ Structuration de l'annuaire Active Directory

> Organisation du domaine raidaporter.local en OU par service métier, avec groupes de sécurité à trois niveaux de droits.

## Organisation du domaine

Le domaine matérialise l'organigramme de RAID-A-PORTER : direction générale + 5 directions opérationnelles (Finance, Commerce, Technique, RH, Logistique) + direction Informatique, pour ~60 comptes utilisateurs.


## [Durcissement appliqué](https://www.it-connect.fr/comment-creer-un-domaine-active-directory-respectueux-des-bonnes-pratiques-de-securite/#google_vignette)

Le déploiement initial de l'AD a été **repris en cours de projet** pour appliquer les bonnes pratiques : compte d'administration nominatif distinct du compte par défaut, mot de passe fort sur le compte de délégation, stratégie de mots de passe robuste.

<img width="1533" height="1000" alt="pingcastle" src="https://github.com/user-attachments/assets/fb2ac804-e6b2-47d7-b554-d412ad2f0d6a" />


## Structuration des OU

L'OU racine `RAIDAPORTER` regroupe deux sous-OU : `Utilisateurs` et `Ordinateurs`. Chaque service métier possède sa propre OU, préfixée numériquement pour le tri, avec 3 groupes de sécurité :

- `_R` (Read) — lecture seule
- `_M` (Modify) — lecture + modification
- `_G` (Gestion) — administration des objets du service

```powershell
New-ADOrganizationalUnit -Name '110000_SCE_FINANCE' `
  -Path 'OU=Utilisateurs,OU=RAIDAPORTER,DC=raidaporter,DC=local' `
  -ProtectedFromAccidentalDeletion $true
```
<img width="1149" height="655" alt="ad" src="https://github.com/user-attachments/assets/870c07d8-a18f-49fa-8ba5-2ca1c1af9bc6" />

## Incident résolu

Un **renommage à chaud d'un contrôleur de domaine** n'est pas supporté et a nécessité un redéploiement complet du DC concerné — leçon retenue pour la suite du projet.

## Repos liés

- `ad-automatisation-powershell`
- `dfs-architecture` — permissions NTFS alignées sur ces groupes
- `orangehrm-gestion-comptes` — consommateur de l'annuaire

## Auteur

**Lilian Vertueux** — [LinkedIn](https://www.linkedin.com/in/lilian-vertueux/)
