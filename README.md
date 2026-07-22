# 🗂️ Structuration de l'annuaire Active Directory

> Organisation du domaine raidaporter.local en OU par service métier, avec groupes de sécurité à trois niveaux de droits.

## Organisation du domaine

Le domaine matérialise l'organigramme de RAID-A-PORTER : direction générale + 5 directions opérationnelles (Finance, Commerce, Technique, RH, Logistique) + direction Informatique, pour ~60 comptes utilisateurs.

**3 contrôleurs de domaine** assurent la redondance des rôles FSMO et de la résolution DNS : `h-winserv-1`, `h-winserv-2`, `lap-winserv-1`.

## Durcissement appliqué

Le déploiement initial de l'AD a été **repris en cours de projet** pour appliquer les bonnes pratiques : compte d'administration nominatif distinct du compte par défaut, mot de passe fort sur le compte de délégation, stratégie de mots de passe robuste.

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

## Incident résolu

Un **renommage à chaud d'un contrôleur de domaine** n'est pas supporté et a nécessité un redéploiement complet du DC concerné — leçon retenue pour la suite du projet.

## Repos liés

- `ad-automatisation-powershell`
- `dfs-architecture` — permissions NTFS alignées sur ces groupes
- `orangehrm-gestion-comptes` — consommateur de l'annuaire

## Auteur

**Lilian Vertueux** — [LinkedIn](https://www.linkedin.com/in/lilian-vertueux/)
