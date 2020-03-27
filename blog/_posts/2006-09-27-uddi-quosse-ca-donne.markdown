---
author: ixe013
comments: false
date: 2006-09-27 14:39:38+00:00
layout: post
link: http://www.paralint.com/blog/2006/09/27/uddi-quosse-ca-donne/
slug: uddi-quosse-ca-donne
title: UDDI, qu’osse ça donne ?
wordpress_id: 8
categories:
- En français
- Web Services
---

J'ai lu [la présentation sur les services web](http://www.benoitpiette.com/blogue/index.php/2006/09/16/106-video---introduction-aux-web-services) de Benoit Piette. Très bien, mais j'ai des réserves sur tout ce qui est UDDI. Non pas que la technologie est mal expliquée, c'est juste que je résume habituellement UDDI par "technology waiting for a problem". Voici mon raisonnement.

Le WSDL n'est pas "sémantique". Surtout pas pour un programme qui ira le lire. Autrement dit, il faut déjà savoir ce qu'on cherche pour le trouver. Il faut avoir une implémentation du client déjà prête. On peut générer un client _on the fly_ à partir du WSDL, mais comment savoir dans quelle ordre appellé les services, que passer en paramètre ? Ainsi, si on a un client, on a probablement déjà le WSDL. Alors comme source de WSDL inconnus, on oublie UDDI.

UDDI pourrais alors à trouver différentes implémentations du service ? Oui, mais ça ne peut marcher qu'a deux conditions qui ne sont pratiquement jamais réunies.



	
  1. L'implémentation doit être standardisée

	
  2. Le service doit être gratuit


Disons que le point 1 arrivera bien un jour, en commençant par des domaines spécialisés et soumis à des réglementations internationale (de l'industrie) comme la réservation de billets d'avion par exemple. D'où le "waiting" de mon résumé UDDI.

Le point 2 est là où tout s'effondre. Si le service n'est pas gratuit, il y aura une entente contractuelle entre les parties avant le premier appel SOAP. Que cette entente prenne la forme d'un contrat écrit, d'un courriel ou même d'un service web d'abonnement, on ne s'en sauve pas. Il y a une étape "contractuelle". Le WSDL (et le endpoint) seront donnés lors de l'établissement du contrat. Pourquoi retourner dans UDDI pour me faire dire ce que je sais déjà ? Le service déménage ? 302 Moved.

UDDI pourra peut-être un jour être la solution au problème qu'il cherche/invente (j'appelle ça la solution "consultant"). Avoir un standard d'établissement de contrat avec un service, c'est la pierre d'achoppement de l'établissement d'un modèle distribué dynamique où UDDI aurait un rôle à jouer. Dans ce modèle, le client cherche les providers de contrats (le service pourrais être son propre provider de contrat), on établis un contrat et on appelle le service. Des standards comme WS-Trust pourrais jouer un rôle dans un tel scénario.
