# ube-uptime

Maintient éveillés les backends Render (offre gratuite) de l'Université de Bertoua.

## Pourquoi ce dépôt existe

Render arrête un service gratuit après ~15 min sans trafic. Le visiteur
suivant paie un réveil de 30 à 60 secondes. Sur UBE Présences, ce délai
dépassait le timeout de l'application : l'écran de connexion affichait
« mot de passe incorrect » alors que les identifiants étaient corrects.

## Pourquoi un dépôt public séparé

Les minutes GitHub Actions sont **illimitées sur un dépôt public**, alors
qu'un dépôt privé n'a que 2000 minutes/mois. Or GitHub facture **une minute
entière par exécution**, même si le ping dure 2 secondes :

| | |
|---|---|
| Pings par mois (toutes les 10 min) | 4320 |
| Minutes facturées | ~4320 |
| Quota gratuit d'un dépôt privé | 2000 |
| **Quota épuisé vers le** | **jour 14 de chaque mois** |

Passé cette date, les exécutions planifiées s'arrêtent **sans aucune
alerte** et le backend recommence à s'endormir. C'est précisément ce qui
est arrivé quand le ping vivait dans le dépôt privé `ube-presence`.

Ce dépôt ne contient donc **aucun code source et aucun secret** — seulement
des URL de santé publiques. Il peut rester public sans risque.

## Services surveillés

| Service | URL pingée |
|---------|-----------|
| UBE Présences | `https://ube-presence-backend.onrender.com/health` |
| OrientCam | `https://orientcam-platform.onrender.com/health` |

## Vérifier que ça tourne

Onglet **Actions** → *Keep Render backends warm*. Une exécution verte
toutes les 10 minutes. Le bouton **Run workflow** force un réveil immédiat,
utile juste avant une démonstration.

> Les exécutions planifiées GitHub peuvent être retardées de quelques
> minutes aux heures de forte charge. C'est normal et sans conséquence ici.

## Battement mensuel

GitHub désactive les workflows planifiés d'un dépôt sans activité depuis
60 jours. Comme rien n'est jamais commité ici, le ping mourrait au bout de
deux mois. Le workflow commite donc `last-heartbeat.txt` le 1er de chaque
mois pour garder le dépôt actif.
