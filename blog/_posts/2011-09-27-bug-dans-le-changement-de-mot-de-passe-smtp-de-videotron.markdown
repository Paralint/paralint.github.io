---
author: ixe013
comments: false
date: 2011-09-27 03:55:20+00:00
layout: post
link: http://www.paralint.com/blog/2011/09/26/bug-dans-le-changement-de-mot-de-passe-smtp-de-videotron/
slug: bug-dans-le-changement-de-mot-de-passe-smtp-de-videotron
title: Bug dans le changement de mot de passe SMTP de Videotron
wordpress_id: 170
categories:
- En français
- Other technical
---

Le site de support à la clientèle de Vidétron offre la possibilité de changer le mot de passe STMP ou POP associé à votre compte. Ce mot de passe n’est pas le même que celui utilisé pour ouvrir une session dans l’espace client. Votre code d’utilisateur débute par VL (en minuscule, pour Videotron lté) vlxxxxxx et vous avez un mot de passe associé pour la réception de courriel SMTP.

J’ai eu à changer ce mot de passe et j’ai bloqué longtemps sur le problème suivant. Et non, je n’ai pas essayé de contacter le support technique à ce sujet, des plans pour qu’ils me demandent de formatter mon PC.

En fait, c’est tout simple. Lorsque vous entrez votre mot de passe dans ce formulaire les majuscules sont converties en minuscules, tout simplement.:

![ntd3eFUc](http://www.paralint.com/blog/wp-content/uploads/2011/09/ntd3eFUc.jpg)

Je me suis douté de quelque chose quand les mots de passe générés par Password Safe ne fonctionnais pas, mais les blasphèmes et insultes marchait tout le temps… Pas de majuscules !

Alors allez vous choisir un nouveau mot de passe SMTP, tout en minuscules. Nous savons tous qu’il n’a pas changé depuis 5 ans au moins !
