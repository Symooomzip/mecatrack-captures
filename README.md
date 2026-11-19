# MécaTrack

**Gestion d'un garage automobile : un ticket par voiture, des photos, une signature, une caisse qui se ferme le soir.**

> **English summary.** MécaTrack is a French-language, installable web app (PWA) that helps small auto repair garages keep their numbers straight. Every car gets a numbered ticket opened on arrival; before/after photos are expected; every payment is signed by the customer on a counter tablet; discounts and second-hand parts need the owner's approval; the evening cash count sets fixed costs aside first. It works offline on a mid-range Android phone, and the notices are doubled in Moroccan Arabic (darija). This repository holds **screenshots only**: 64 screens across 5 user spaces, taken on a demo garage filled with fictional data. The source code is private.
>
> **Stack:** Next.js 15 (App Router, Server Actions) · React 19 · TypeScript · Tailwind CSS 4 · Supabase (PostgreSQL, Auth, Row Level Security) · Cloudflare R2 · Vercel · Vitest.

Toutes les images ont été prises sur un **garage de démonstration** : les noms, les plaques, les montants et les numéros de téléphone sont fictifs, et les photos sont des illustrations générées. Ce dépôt ne contient **aucun code**.

---

## À quoi ça sert

Dans un petit garage, l'argent circule à la main : espèces, achats de pièces, dépenses. À la fin du mois, les charges fixes (loyer, électricité, eau) ne sont pas toujours couvertes, et personne ne sait vraiment où est passé le reste.

MécaTrack ne cherche pas à accuser quelqu'un. Il rend simplement **visible** ce qui se passe, et change les habitudes :

- chaque voiture a un **ticket numéroté**, ouvert à son arrivée, fermé quand le client paie ;
- les **photos** d'entrée, d'avant et d'après montrent ce qui a été fait ;
- le **client signe** le montant qu'il paie ;
- une **remise** est une ligne à part, et au-delà d'un seuil le patron doit l'accepter ;
- une **pièce d'occasion** (la casse, où il n'existe pas de facture) demande l'accord du patron *avant* l'achat, avec le prix habituel, le reçu manuscrit photographié et le téléphone du vendeur ;
- les **charges fixes** sont mises de côté chaque jour, avant tout le reste ;
- les mécaniciens touchent une **commission sur la main-d'œuvre**, jamais sur les pièces : plus aucun intérêt à gonfler un prix.

## Le parcours d'une voiture

<table>
<tr>
<td align="center" valign="top" width="20%"><img src="captures/mecanicien/15-nouveau-ticket-rempli.png" width="150" alt="1"><br><sub><b>1. Elle arrive</b><br>Le mécanicien ouvre un ticket (objectif : moins de 90 secondes).</sub></td>
<td align="center" valign="top" width="20%"><img src="captures/mecanicien/16-ticket-ouvert.png" width="150" alt="2"><br><sub><b>2. On la répare</b><br>Photos, travaux, montants, pièces demandées.</sub></td>
<td align="center" valign="top" width="20%"><img src="captures/mecanicien/24-demander-une-remise-accord-patron.png" width="150" alt="3"><br><sub><b>3. Une remise ?</b><br>Au-delà du seuil, le patron répond depuis son téléphone.</sub></td>
<td align="center" valign="top" width="20%"><img src="captures/comptoir/31-le-client-paie-signature.png" width="150" alt="4"><br><sub><b>4. Le client paie</b><br>Au comptoir, il signe le montant au doigt.</sub></td>
<td align="center" valign="top" width="20%"><img src="captures/comptoir/36-caisse-fermee.png" width="150" alt="5"><br><sub><b>5. On ferme la caisse</b><br>Écart expliqué, reçu de versement, charges mises de côté.</sub></td>
</tr>
</table>

Le soir, le patron, même à l'étranger, ouvre **Aujourd'hui** : ce qui a été encaissé, les voitures qui traînent, les alertes.

<p align="center"><a href="captures/patron/37-aujourdhui.png"><img src="captures/patron/37-aujourdhui.png" width="760" alt="Tableau de bord du patron"></a></p>

## Ce qui a été pensé pour le terrain

- **Un téléphone, une main.** Chaque écran du mécanicien se manie au pouce sur un Android milieu de gamme, avec de grosses zones à toucher et un minimum de champs.
- **Sans réseau.** Le garage perd la connexion : les tickets et les photos sont gardés sur le téléphone, puis envoyés au retour du réseau. Le numéro de ticket reste donné par le serveur, sans trou.
- **Français simple, et darija.** Le texte est celui d'un garage (« La voiture est arrivée », « Combien le client a payé ? »), doublé en darija en lettres arabes sous les notices.
- **Les règles sont dans la base.** Rien n'est jamais supprimé : une correction crée une trace avec son motif, dans un journal d'audit immuable. Un mécanicien ne peut pas encaisser, signer à la place du client, ni voir le garage d'un autre.
- **Plusieurs garages.** L'application est multi-garages : essai de 2 mois, abonnement payé à la main (virement, CashPlus, espèces), lecture seule à l'expiration. L'administrateur de la plateforme ne voit jamais le contenu d'un garage.

---

## Les écrans

### Visiteur

Ce que voit un garagiste avant de s'inscrire, et l'écran de connexion.

<table>
<tr>
<td align="center" valign="top" width="25%"><a href="captures/public/01-presentation-ordinateur.png"><img src="captures/public/01-presentation-ordinateur.png" width="190" alt="Présentation : la promesse et les formules (ordinateur)"></a><br><sub>Présentation : la promesse et les formules (ordinateur)</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/public/02-presentation-telephone.png"><img src="captures/public/02-presentation-telephone.png" width="190" alt="La même page sur téléphone"></a><br><sub>La même page sur téléphone</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/public/03-inscription.png"><img src="captures/public/03-inscription.png" width="190" alt="Inscrire son garage : 2 mois gratuits, aucun paiement"></a><br><sub>Inscrire son garage : 2 mois gratuits, aucun paiement</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/public/04-inscription-erreurs.png"><img src="captures/public/04-inscription-erreurs.png" width="190" alt="Les erreurs s'affichent toutes ensemble, en français simple"></a><br><sub>Les erreurs s'affichent toutes ensemble, en français simple</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="25%"><a href="captures/public/05-connexion.png"><img src="captures/public/05-connexion.png" width="190" alt="Connexion par numéro de téléphone"></a><br><sub>Connexion par numéro de téléphone</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/public/06-connexion-erreur.png"><img src="captures/public/06-connexion-erreur.png" width="190" alt="Le même message pour un numéro ou un mot de passe faux"></a><br><sub>Le même message pour un numéro ou un mot de passe faux</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/public/07-hors-ligne.png"><img src="captures/public/07-hors-ligne.png" width="190" alt="Page affichée quand il n'y a pas de réseau"></a><br><sub>Page affichée quand il n'y a pas de réseau</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/public/08-page-introuvable.png"><img src="captures/public/08-page-introuvable.png" width="190" alt="Page introuvable"></a><br><sub>Page introuvable</sub></td>
</tr>
</table>

### Mécanicien (téléphone)

90 % de l'usage : ouvrir un ticket, prendre des photos, demander une pièce ou une remise, à une main sur un Android milieu de gamme.

<table>
<tr>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/09-premier-lancement-1.png"><img src="captures/mecanicien/09-premier-lancement-1.png" width="190" alt="Premier lancement 1/3 : une voiture arrive, ouvrez un ticket"></a><br><sub>Premier lancement 1/3 : une voiture arrive, ouvrez un ticket</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/10-premier-lancement-2.png"><img src="captures/mecanicien/10-premier-lancement-2.png" width="190" alt="Premier lancement 2/3 : prenez trois photos"></a><br><sub>Premier lancement 2/3 : prenez trois photos</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/11-premier-lancement-3.png"><img src="captures/mecanicien/11-premier-lancement-3.png" width="190" alt="Premier lancement 3/3 : le patron voit tout, le soir"></a><br><sub>Premier lancement 3/3 : le patron voit tout, le soir</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/12-accueil-invitation-installer.png"><img src="captures/mecanicien/12-accueil-invitation-installer.png" width="190" alt="Invitation à ajouter l'application à l'écran d'accueil"></a><br><sub>Invitation à ajouter l'application à l'écran d'accueil</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/13-accueil-au-garage.png"><img src="captures/mecanicien/13-accueil-au-garage.png" width="190" alt="Les voitures au garage, la plus ancienne en haut ; en rouge au bout de 7 jours"></a><br><sub>Les voitures au garage, la plus ancienne en haut ; en rouge au bout de 7 jours</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/14-nouveau-ticket.png"><img src="captures/mecanicien/14-nouveau-ticket.png" width="190" alt="« La voiture est arrivée » : ouverture d'un ticket"></a><br><sub>« La voiture est arrivée » : ouverture d'un ticket</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/15-nouveau-ticket-rempli.png"><img src="captures/mecanicien/15-nouveau-ticket-rempli.png" width="190" alt="Plaque, téléphone, panne en une pastille : l'objectif est moins de 90 secondes"></a><br><sub>Plaque, téléphone, panne en une pastille : l'objectif est moins de 90 secondes</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/16-ticket-ouvert.png"><img src="captures/mecanicien/16-ticket-ouvert.png" width="190" alt="Fiche d'un ticket : photos, travaux, montants, pièces"></a><br><sub>Fiche d'un ticket : photos, travaux, montants, pièces</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/17-ticket-pret-remise-refusee.png"><img src="captures/mecanicien/17-ticket-pret-remise-refusee.png" width="190" alt="Ticket prêt ; la remise refusée par le patron reste visible"></a><br><sub>Ticket prêt ; la remise refusée par le patron reste visible</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/18-ticket-remise-en-attente.png"><img src="captures/mecanicien/18-ticket-remise-en-attente.png" width="190" alt="Remise en attente : le comptoir ne peut pas encaisser"></a><br><sub>Remise en attente : le comptoir ne peut pas encaisser</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/19-ticket-paye.png"><img src="captures/mecanicien/19-ticket-paye.png" width="190" alt="Ticket payé : plus rien ne se modifie"></a><br><sub>Ticket payé : plus rien ne se modifie</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/20-ticket-annule.png"><img src="captures/mecanicien/20-ticket-annule.png" width="190" alt="Ticket annulé, avec son motif (rien n'est jamais supprimé)"></a><br><sub>Ticket annulé, avec son motif (rien n'est jamais supprimé)</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/21-demander-une-piece.png"><img src="captures/mecanicien/21-demander-une-piece.png" width="190" alt="Demander une pièce"></a><br><sub>Demander une pièce</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/22-demander-une-piece-occasion.png"><img src="captures/mecanicien/22-demander-une-piece-occasion.png" width="190" alt="Pièce d'occasion : accord du patron, prix habituel, reçu du vendeur"></a><br><sub>Pièce d'occasion : accord du patron, prix habituel, reçu du vendeur</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/23-demander-une-remise.png"><img src="captures/mecanicien/23-demander-une-remise.png" width="190" alt="Demander une remise : un montant et un motif à toucher"></a><br><sub>Demander une remise : un montant et un motif à toucher</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/24-demander-une-remise-accord-patron.png"><img src="captures/mecanicien/24-demander-une-remise-accord-patron.png" width="190" alt="Au-delà de 200 MAD ou 15 % : il faut l'accord du patron"></a><br><sub>Au-delà de 200 MAD ou 15 % : il faut l'accord du patron</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/25-photos-manquantes.png"><img src="captures/mecanicien/25-photos-manquantes.png" width="190" alt="« Prête » sans photos : possible, mais le patron est prévenu"></a><br><sub>« Prête » sans photos : possible, mais le patron est prévenu</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/26-a-envoyer.png"><img src="captures/mecanicien/26-a-envoyer.png" width="190" alt="File d'envoi : ce qui est gardé sur le téléphone sans réseau"></a><br><sub>File d'envoi : ce qui est gardé sur le téléphone sans réseau</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/27-acces-reserve.png"><img src="captures/mecanicien/27-acces-reserve.png" width="190" alt="Un écran réservé au patron reste fermé au mécanicien"></a><br><sub>Un écran réservé au patron reste fermé au mécanicien</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/mecanicien/28-acces-reserve-comptoir.png"><img src="captures/mecanicien/28-acces-reserve-comptoir.png" width="190" alt="L'encaissement se fait au comptoir, pas sur le téléphone du mécanicien"></a><br><sub>L'encaissement se fait au comptoir, pas sur le téléphone du mécanicien</sub></td>
</tr>
</table>

### Comptoir (tablette)

Une tablette partagée au comptoir : le client paie et signe lui-même, puis on ferme la caisse le soir.

<table>
<tr>
<td align="center" valign="top" width="25%"><a href="captures/comptoir/29-a-encaisser.png"><img src="captures/comptoir/29-a-encaisser.png" width="190" alt="À encaisser : les voitures prêtes ; les autres sont grisées avec la raison"></a><br><sub>À encaisser : les voitures prêtes ; les autres sont grisées avec la raison</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/comptoir/30-le-client-paie.png"><img src="captures/comptoir/30-le-client-paie.png" width="190" alt="Le client paie : le détail, le total, le mode de paiement"></a><br><sub>Le client paie : le détail, le total, le mode de paiement</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/comptoir/31-le-client-paie-signature.png"><img src="captures/comptoir/31-le-client-paie-signature.png" width="190" alt="Le client signe au doigt le montant qu'il paie"></a><br><sub>Le client signe au doigt le montant qu'il paie</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/comptoir/32-encaisse-ticket-imprimable.png"><img src="captures/comptoir/32-encaisse-ticket-imprimable.png" width="190" alt="Ticket client prêt pour l'imprimante thermique"></a><br><sub>Ticket client prêt pour l'imprimante thermique</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="25%"><a href="captures/comptoir/33-caisse-du-jour.png"><img src="captures/comptoir/33-caisse-du-jour.png" width="190" alt="Caisse du jour : fond, espèces encaissées, charges à mettre de côté"></a><br><sub>Caisse du jour : fond, espèces encaissées, charges à mettre de côté</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/comptoir/34-caisse-ecart-et-versement.png"><img src="captures/comptoir/34-caisse-ecart-et-versement.png" width="190" alt="Compté, écart à expliquer, montant versé à la banque"></a><br><sub>Compté, écart à expliquer, montant versé à la banque</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/comptoir/35-caisse-recu-de-versement.png"><img src="captures/comptoir/35-caisse-recu-de-versement.png" width="190" alt="Photo du reçu de versement à la banque"></a><br><sub>Photo du reçu de versement à la banque</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/comptoir/36-caisse-fermee.png"><img src="captures/comptoir/36-caisse-fermee.png" width="190" alt="Caisse fermée : plus rien ne peut être modifié"></a><br><sub>Caisse fermée : plus rien ne peut être modifié</sub></td>
</tr>
</table>

### Patron (ordinateur et téléphone)

Le tableau de bord, les approbations et les alertes, consultables de loin.

<table>
<tr>
<td align="center" valign="top" width="50%"><a href="captures/patron/37-aujourdhui.png"><img src="captures/patron/37-aujourdhui.png" width="470" alt="Aujourd'hui : alertes, argent encaissé, charges mises de côté, voitures au garage"></a><br><sub>Aujourd'hui : alertes, argent encaissé, charges mises de côté, voitures au garage</sub></td>
<td align="center" valign="top" width="50%"><a href="captures/patron/38-approbations.png"><img src="captures/patron/38-approbations.png" width="470" alt="Approbations : pièces d'occasion et remises qui attendent le patron"></a><br><sub>Approbations : pièces d'occasion et remises qui attendent le patron</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="50%"><a href="captures/patron/39-alertes.png"><img src="captures/patron/39-alertes.png" width="470" alt="Alertes : écart de caisse, prix hors fourchette, stock bas, ticket trop ancien"></a><br><sub>Alertes : écart de caisse, prix hors fourchette, stock bas, ticket trop ancien</sub></td>
<td align="center" valign="top" width="50%"><a href="captures/patron/40-tous-les-tickets.png"><img src="captures/patron/40-tous-les-tickets.png" width="470" alt="Tous les tickets, avec recherche et détection des numéros manquants"></a><br><sub>Tous les tickets, avec recherche et détection des numéros manquants</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="50%"><a href="captures/patron/41-fiche-ticket-paye.png"><img src="captures/patron/41-fiche-ticket-paye.png" width="470" alt="Fiche d'un ticket payé : photos, montants, correction par le patron seul"></a><br><sub>Fiche d'un ticket payé : photos, montants, correction par le patron seul</sub></td>
<td align="center" valign="top" width="50%"><a href="captures/patron/42-fiche-ticket-historique.png"><img src="captures/patron/42-fiche-ticket-historique.png" width="470" alt="Historique d'un ticket : qui a changé quoi, et quand"></a><br><sub>Historique d'un ticket : qui a changé quoi, et quand</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="50%"><a href="captures/patron/43-fiche-ticket-ancien.png"><img src="captures/patron/43-fiche-ticket-ancien.png" width="470" alt="Un ticket resté 9 jours au garage"></a><br><sub>Un ticket resté 9 jours au garage</sub></td>
<td align="center" valign="top" width="50%"><a href="captures/patron/44-stock.png"><img src="captures/patron/44-stock.png" width="470" alt="Stock : seuils d'alerte et inventaire"></a><br><sub>Stock : seuils d'alerte et inventaire</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="50%"><a href="captures/patron/45-fournisseurs.png"><img src="captures/patron/45-fournisseurs.png" width="470" alt="Fournisseurs officiels et casses"></a><br><sub>Fournisseurs officiels et casses</sub></td>
<td align="center" valign="top" width="50%"><a href="captures/patron/46-fournisseur-fiche.png"><img src="captures/patron/46-fournisseur-fiche.png" width="470" alt="Fiche d'un fournisseur : achats et justificatifs"></a><br><sub>Fiche d'un fournisseur : achats et justificatifs</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="50%"><a href="captures/patron/47-prix-de-reference.png"><img src="captures/patron/47-prix-de-reference.png" width="470" alt="Prix de référence : fourchette habituelle de chaque pièce"></a><br><sub>Prix de référence : fourchette habituelle de chaque pièce</sub></td>
<td align="center" valign="top" width="50%"><a href="captures/patron/48-charges-fixes.png"><img src="captures/patron/48-charges-fixes.png" width="470" alt="Charges fixes : mises de côté chaque jour, avant tout le reste"></a><br><sub>Charges fixes : mises de côté chaque jour, avant tout le reste</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="50%"><a href="captures/patron/49-equipe.png"><img src="captures/patron/49-equipe.png" width="470" alt="Équipe : mécaniciens et compte du comptoir"></a><br><sub>Équipe : mécaniciens et compte du comptoir</sub></td>
<td align="center" valign="top" width="50%"><a href="captures/patron/50-equipe-ajouter.png"><img src="captures/patron/50-equipe-ajouter.png" width="470" alt="Ajouter un mécanicien ou la tablette du comptoir"></a><br><sub>Ajouter un mécanicien ou la tablette du comptoir</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="50%"><a href="captures/patron/51-commissions.png"><img src="captures/patron/51-commissions.png" width="470" alt="Commissions : sur la main-d'œuvre et le nombre de tickets, jamais sur les pièces"></a><br><sub>Commissions : sur la main-d'œuvre et le nombre de tickets, jamais sur les pièces</sub></td>
<td align="center" valign="top" width="50%"><a href="captures/patron/52-abonnement.png"><img src="captures/patron/52-abonnement.png" width="470" alt="Abonnement : essai gratuit et formules"></a><br><sub>Abonnement : essai gratuit et formules</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="50%"><a href="captures/patron/53-comment-ca-marche.png"><img src="captures/patron/53-comment-ca-marche.png" width="470" alt="Aide : comment ça marche"></a><br><sub>Aide : comment ça marche</sub></td>
<td width="50%"></td>
</tr>
</table>

### Patron sur téléphone

Les mêmes écrans, en une colonne, avec une barre d'onglets en bas.

<table>
<tr>
<td align="center" valign="top" width="25%"><a href="captures/patron/54-mobile-aujourdhui.png"><img src="captures/patron/54-mobile-aujourdhui.png" width="190" alt="Aujourd'hui"></a><br><sub>Aujourd'hui</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/patron/55-mobile-tickets.png"><img src="captures/patron/55-mobile-tickets.png" width="190" alt="Tous les tickets"></a><br><sub>Tous les tickets</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/patron/56-mobile-approbations.png"><img src="captures/patron/56-mobile-approbations.png" width="190" alt="Approbations"></a><br><sub>Approbations</sub></td>
<td align="center" valign="top" width="25%"><a href="captures/patron/57-mobile-plus.png"><img src="captures/patron/57-mobile-plus.png" width="190" alt="Plus : le reste du menu"></a><br><sub>Plus : le reste du menu</sub></td>
</tr>
</table>

### Administrateur de la plateforme

Un compte séparé de tous les garages : il gère les abonnements et voit l'activité, jamais le contenu d'un garage (ni clients, ni montants, ni caisse).

<table>
<tr>
<td align="center" valign="top" width="50%"><a href="captures/plateforme/58-vue-d-ensemble.png"><img src="captures/plateforme/58-vue-d-ensemble.png" width="470" alt="Vue d'ensemble : paiements à confirmer, garages à relancer, revenus"></a><br><sub>Vue d'ensemble : paiements à confirmer, garages à relancer, revenus</sub></td>
<td align="center" valign="top" width="50%"><a href="captures/plateforme/59-garages.png"><img src="captures/plateforme/59-garages.png" width="470" alt="Garages, avec leur état d'abonnement"></a><br><sub>Garages, avec leur état d'abonnement</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="50%"><a href="captures/plateforme/60-garage-detail.png"><img src="captures/plateforme/60-garage-detail.png" width="470" alt="Un garage : abonnement, activité, activation d'une formule"></a><br><sub>Un garage : abonnement, activité, activation d'une formule</sub></td>
<td align="center" valign="top" width="50%"><a href="captures/plateforme/61-garage-detail-paiement-en-attente.png"><img src="captures/plateforme/61-garage-detail-paiement-en-attente.png" width="470" alt="Un garage qui a déclaré un paiement"></a><br><sub>Un garage qui a déclaré un paiement</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="50%"><a href="captures/plateforme/62-paiements-a-confirmer.png"><img src="captures/plateforme/62-paiements-a-confirmer.png" width="470" alt="Paiements à confirmer (virement, CashPlus, espèces)"></a><br><sub>Paiements à confirmer (virement, CashPlus, espèces)</sub></td>
<td align="center" valign="top" width="50%"><a href="captures/plateforme/63-formules-et-prix.png"><img src="captures/plateforme/63-formules-et-prix.png" width="470" alt="Formules et prix, modifiables sans toucher au code"></a><br><sub>Formules et prix, modifiables sans toucher au code</sub></td>
</tr>
<tr>
<td align="center" valign="top" width="50%"><a href="captures/plateforme/64-mobile-vue-d-ensemble.png"><img src="captures/plateforme/64-mobile-vue-d-ensemble.png" width="470" alt="Vue d'ensemble sur téléphone"></a><br><sub>Vue d'ensemble sur téléphone</sub></td>
<td width="50%"></td>
</tr>
</table>

---

## À propos de ces captures

- Les captures sont faites par un script, en haute définition, sur un garage de démonstration créé pour l'occasion puis supprimé. Avant chaque capture, tout ce qui ressemblerait à une vraie donnée est remplacé, et la capture s'arrête si c'est impossible.
- Les photos de voitures et les reçus sont des **illustrations dessinées** pour la démonstration.
- Le code source est dans un dépôt privé.

© 2026. Tous droits réservés.
