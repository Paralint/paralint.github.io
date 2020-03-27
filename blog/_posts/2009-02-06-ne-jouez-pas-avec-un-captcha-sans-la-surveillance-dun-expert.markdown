---
author: ixe013
comments: false
date: 2009-02-06 20:22:52+00:00
layout: post
link: http://www.paralint.com/blog/2009/02/06/ne-jouez-pas-avec-un-captcha-sans-la-surveillance-dun-expert/
slug: ne-jouez-pas-avec-un-captcha-sans-la-surveillance-dun-expert
title: Ne jouez pas avec un CAPTCHA sans la surveillance d'un expert
wordpress_id: 91
categories:
- En français
- Other technical
- Security
---

Je suis tombé sur cette implémentation d'un CAPTCHA.


![Must select 3 hamburgers in this lousy captcha ](http://www.paralint.com/blog/wp-content/uploads/2009/02/captcha.jpg)


Je déteste les CAPTCHA. C'est comme de la mauvaise crypto.

Fondamentalement, le CAPTCHA ne fonctionne pas. La tâche d'analyse (le test de Turing) est complexe juste parce que personne ne s'est encore donné la peine d'écrire le code pour réussir. C'est aussi vrai pour la crypto classique, mais ces mathématiques sont soumises à des études formelles et continues. On sait à quoi s'en tenir : avec de la bonne crypto, on déplace le problème ailleurs (la gestion de clé, souvent). En intelligence artificielle, la segmentation est difficile, mais l'ordre de grandeur d'effort est à la portée des botnets actuels.

<!-- more -->Trouver un éléphant ou un burger est faisable. Google ne trouve-t-il pas déjà les visages sur les photos ?  La faiblesse de ce CAPTCHA, en particulier, c'est l'apprentissage (en supposant que l'implémentation est bonne). La mémoire d'un ordinateur est infinie. Il est possible d'avoir la base de données complète des images en relativement peu de temps. D'identifier ce qui s'y trouve et automatiser le tout. Bien sûr, il ne faut pas que l'image soit déjà dans l'index Google... Autre faiblesse, on sait toujours d'avance combien il y a de (chat-chien-burger-bébé-éléphant). Ça aide à prendre une décision automatisée.

Les attaques sur les CAPTCHA de Microsoft et Google a montré que l'analyse de CAPTCHA n'a pas un taux de succès de 100 %. Un petit pourcentage, multiplié par un bon botnet, ça fait beaucoup de captcha résolus!

On n'a qu'à ajouter de nouvelles images, non? Oui, mais combien coûtera toute cette mécanique, en développement mais surtout en entretien? Quel est le coût réel d'une utilisation abusive du service? Combien coutera l'analyse préalable des images, le classement en mots clés, etc.? On m'a demandé de compter 3 bébés, mais une image en contenait deux, faut-il filtrer ces images-là aussi?

Mais surtout, l'aspect économique de la sécurité est complètement évacué. Ce CAPTCHA est sur une page qui demande un numéro de carte de crédit, mais pas sur la page qui permet d'utiliser le service une seule fois, gratuitement.  Et c'est sans compter qu'une [ferme de Turing](http://decapcher.com) coute 8 $ pour 4000 CAPTCHA résolus. Combien coûte un client légitime dégoûté par toutes ces précautions?

Ce CAPTCHA est l'œuvre d'amateurs. Et j'ai même pas regardé l'implémentation des cookies, session, etc.  En bref, implémenter correctement ce CAPTCHA par image coûte beaucoup trop cher.

Si j'étais forcé d'utiliser un CAPTCHA, j'utiliserais un simple encodage javascript des champs du formulaire, variable dans le temps, peut-être avec un [hashcash](http://hashcash.org) en javascript aussi, et vraiment accolé au pied du mur, j'essaierais [recaptcha](http://recaptcha.net). L'idée est de transformer tes utilisateurs en ta propre ferme de Turing. Pas fou!
