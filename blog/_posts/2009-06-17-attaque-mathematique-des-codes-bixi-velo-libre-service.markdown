---
author: ixe013
comments: false
date: 2009-06-17 14:30:12+00:00
layout: post
link: http://www.paralint.com/blog/2009/06/17/attaque-mathematique-des-codes-bixi-velo-libre-service/
slug: attaque-mathematique-des-codes-bixi-velo-libre-service
title: Attaque mathématique des codes Bixi (vélo libre-service)
wordpress_id: 119
categories:
- En français
- Life (the real one)
- Math
---

J'étais au centre ville aujourd'hui et j'avais affaire pas très loin. Au lieu d'utiliser le métro, j'ai loué un Bixi, vélo en libre service. J'ai été surpris de constater que les codes Bixi ne comporte que 3 chiffres, sur 5 caractères, soit 3^5=243 codes possibles.

[caption id="attachment_120" align="aligncenter" width="417" caption="Billet bixi portant le code 21213"]![Billet bixi portant le code 21213](/blog/images/{{ page.date | date: "%Y-%m-%d" }}-{{ page.slug }}/bixi21213.jpg)[/caption]

Ça semble bien peu, mais si les codes ne sont utilisables qu'au point de service où ils sont émis, et pour une durée limitée, avec peut-être une détection d'attaque en force brute, on devrais pouvoir dormir tranquille...

Je me suis rappellé une attaque mathémaitque sur des codes de ce genre. Avec une location de 24 heures à 5$, on peut prendre et remettre le vélo plusieurs fois, histoire de tester la théorie... Bonne nouvelle, Bixi n'est pas vulnérable. Mais j'écris quand même la démarche, c'est trop rare qu'on a l'occasion d'utiliser des math pour (tenter de) contourner les règles d'un système.

<!-- more -->La vulnérabilité apparais lorsque l'implémentation utilise une fenêtre coulissante pour vérifier les codes entrés. Cette attaque a déjà été utilisée pour [dévérouiller une porte de voiture](http://everything2.com/index.pl?node_id=1520430) à combinaison.

Pour les Bixi, les chiffres du codes de dévérouillage de vélo peuvent être 1, 2 ou 3. Le dictionnaire des valeurs possibles à une taille k=3. Il faut entrer 5 chiffres, on dit que n=5. Avec ces variables, une fenêtre coulissante fonctionne comme ceci :



	
  1. On entre n chiffres (par exemple 12222)

	
  2. Ce ne sera pas le bon code

	
  3. On entre un chiffre supplémentaire, disons 3

	
  4. Pour accomoder le chiffre supplémentaire, l'implémentation décale les chiffres déjà entrés pour faire de la place. Dans notre exemple, c'est le 1 qui disparaît à gauche à la faveur du 3 à droite.

	
  5. L'implémentation vérifie le nouveau code dans la fenêtre, soit 22223.


On réussi ainsi à tester 2 codes en ne saisissant que 6 chiffres, au lieu de 10. Et ainsi de suite, à chaque fois qu'on ajoute un seul chiffre, on se trouve à tester un nouveau code de 5 chiffres. La première étape ne sert qu'à initialiser le processus, sans affecter la complexité de l'attaque.

C'est une vielle idée. Un mathématicien Hollandais, Nicolaas Govert de Bruijn, a formalisé une [séquence de chiffres](http://en.wikipedia.org/wiki/De_Bruijn_sequence) dans laquelle n'importe quelle séquence de n chiffres n'apparait qu'une seule fois. Autrement dit, si vous [générez une séquence de Bruijn avec k=3 et n=5](http://www.hakank.org/comb/debruijn.cgi?k=3&n=5) (les codes Bixi) et que vous y cherchez votre code Bixi, il n'y sera qu'une seule fois. C'est la façon la plus optimale d'attaquer un système de codes comme celui de Bruijn.

Voici la séquence dans l'ordre. Elle se lit de gauche à droite, de haut en bas. 1111121111311...... Le 1111 de la fin de la séquence est en fait les premiers chiffres qui se répêtent.

    
    11111 21111 31112 21112 31113 21113 31121 21121 31122 21122



    
    31123 21123 31131 21131 31132 21132 31133 21133 31212 21212



    
    31213 21213 31221 31222 21222 31223 21223 31231 31232 21232



    
    31233 21233 31313 21313 31322 21322 31323 21323 31332 21332



    
    31333 21333 32222 23222 33223 23223 33232 33233 33311 11


La protection contre cette attaque est heureusement toute simple : à l'étape 2, effacer tous les chiffres et en redemander n autres. C'est ce qu'on fait les designers de Bixi. L'histoire ne dit pas s'ils connaissaient l'attaque, ou s'ils ont été chanceux... Au moins il pourrons dire qu'il m'ont fais marcher et pédaler !
