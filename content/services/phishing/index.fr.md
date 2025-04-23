---
title: 'Simulation de Phishing'
type: landing

design:
  spacing: "0rem"

sections:
  - block: hero-rel
    content:
      title: Simulation de Phishing
      text: Saviez-vous que 91% des fuites de données réussies ont commencé par une attaque de spear phishing ?
      secondary_action:
        text: Qu'est-ce qu'une attaque de phishing ?
        url: "#definition"
      announcement:
        text: "Découvrez comment on"
        link:
          text: "phish"
          url: "#process"
    design:
      css_class: "dark"
      background:
        color: "navy"
        image:
          filename: constellation.svg
          size: "auto;background-repeat:repeat"
          filters:
            brightness: 0.5

  - block: cta-image-paragraph-custom
    id: definition
    content:
      items:
        - title: Phishing... c'est quoi ? Une nouvelle technique de pêche ?
          image: phish.svg
          text: |
            Le phishing est un procédé visant à obtenir des informations sensibles telles que des noms d'utilisateur, des mots de passe et des informations de carte de crédit en se faisant passer pour une entité de confiance via l'envoi d'emails en masse qui essaient de contourner les filtres anti-spam.

            Les emails prétendant provenir de réseaux sociaux populaires, de banques, de sites d'ecommerce ou du support informatique sont couramment utilisés pour tromper le public. C'est une forme d'ingénierie sociale frauduleuse et criminelle.

  - block: features
    id: types
    content:
      title: Les types de phishing les plus courants
      text: "Parmi les centaines d'escroqueries de phishing connues, voici les quatre types les plus courants :"
      items:
        - name: E-mail
          icon: envelope
          description: |
            Dans une attaque de phishing par email, un sentiment d'urgence est prédominant. Les escrocs envoient des emails légitimes en apparence à plusieurs destinataires, les encourageant à modifier leurs mots de passe ou à mettre à jour leurs informations personnelles.
        - name: Smishing
          icon: chat-bubble-bottom-center-text
          description: |
            Cette tactique de phishing ressemble de près aux emails de phishing. Les pirates tentent de voler des informations confidentielles en envoyant des messages texte (SMS) insistant pour qu'ils répondent ou fassent certaines actions. Si la personne refuse, les criminels vont parfois jusqu'à la menacer.
        - name: Spear Phishing
          icon: finger-print
          description: |
            Cette tactique utilise des emails pour mener une attaque contre un individu ou une entreprise en particulier. Le criminel acquiert des informations personnelles sur sa cible et les utilise pour lui envoyer un email personnalisé et digne de confiance en apparence.
        - name: Fraude au PDG
          icon: identification
          description: |
            Les cybercriminels envoient des emails en se faisant passer pour un cadre dirigeant ou simplement un collègue, demandant généralement un transfert de fonds ou des informations fiscales.
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"
  
  - block: cta-image-paragraph-custom
    parity: "odd"
    content:
      items:
        - title: Qu'est-ce qu'une simulation de phishing et pourquoi est-ce important ?
          image: laptop-phish.png
          text: |
            Les simulations de phishing sont des imitations d'emails de phishing réels que les organisations peuvent envoyer à leurs employés pour tester leur comportement en ligne et évaluer leur niveau de connaissance des attaques de phishing. Les emails reflètent les cybermenaces que les professionnels peuvent rencontrer dans leurs activités quotidiennes, au travail comme en dehors.

            Les statistiques récentes montrent que les menaces de phishing continuent d'augmenter. Depuis 2019, le nombre d'attaques de phishing a augmenté de 150% par an d'après le groupe Anti-Phishing Working Group (APWG) signalant un record historique en 2022, enregistrant plus de 4,7 millions de sites de phishing. Selon Proofpoint, 84% des organisations en 2022 ont subi au moins une attaque de phishing réussie.

            Parce que même les meilleures firewall email et outils de sécurité ne peuvent pas protéger les organisations de toutes les campagnes de phishing, les organisations se tournent de plus en plus vers les simulations de phishing. Les simulations bien conçues aident à atténuer l'impact des attaques de phishing de deux manières importantes. Elles fournissent aux équipes de sécurité des informations nécessaires pour éduquer les employés à mieux reconnaître et éviter les attaques de phishing réelles. Elles aident également les équipes de sécurité à identifier les vulnérabilités, à améliorer la réponse aux incidents et à réduire les risques de violations de données et de pertes financières dues à des tentatives de phishing réussies.

  - block: defile
    id: process
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"
    content:
      title: Déroulement d'une simulation de phishing
      text: "Cela se déroule généralement cinq étapes :"
      items:
        - name: Planification
          icon: calendar-days
          description: |
            Nous définissons d'abord les objectifs et établissons le périmètre, en décidant du type d'emails de phishing à utiliser et de la fréquence des simulations. Nous déterminerons également le public cible, y compris le ciblage de groupes ou départements spécifiques.
        - name: Rédaction
          icon: document-text
          description: |
            Après avoir élaboré un plan, nous créons des emails de phishing fictifs réalistes qui ressemblent de près à de véritables menaces de phishing, basés sur des modèles de phishing et des kits de phishing disponibles sur le dark web. Nous prêtons une attention particulière aux détails tels que l'objet de l'email, les adresses des expéditeurs et le contenu pour rendre les simulations réalistes. Nous inclurons également des tactiques d'ingénierie sociale, pouvant aller jusqu'à usurper l'identité d'un cadre ou d'un collègue en tant qu'expéditeur, pour augmenter la probabilité que les employés cliquent sur les emails.
        - name: Envoi
          icon: paper-airplane
          description: |
            Une fois le contenu finalisé, nous enverrons les emails de phishing simulés au public cible par des moyens sécurisés, en respectant la confidentialité.
        - name: Suivi
          icon: eye
          description: |
            Après l'envoi des emails malveillants fictifs, nous suivrons et enregistrerons de près la manière dont les employés interagissent avec les emails simulés, en surveillant s'ils cliquent sur les liens, téléchargent les pièces jointes ou fournissent des informations sensibles.
        - name: Analyse
          icon: magnifying-glass
          description: |
            Suite au test de phishing, nous analyserons les données de la simulation pour déterminer des tendances telles que les taux de clics et les vulnérabilités de sécurité. Ensuite, nous ferons un suivi avec les employés ayant échoué à la simulation en leur fournissant un retour immédiat, expliquant comment ils auraient pu identifier correctement la tentative de phishing et comment éviter les attaques réelles à l'avenir.

  - block: cta-card
    content: 
      title: La sensibilisation est le meilleur moyen de prévenir les attaques de phishing
      text: |
        Mettre en œuvre une formation de simulation de phishing est la recette idéale pour renforcer la protection des données. Cela aidera à garder vos employés vigilants et conscients de toutes les escroqueries liées au phishing qu'ils pourraient rencontrer.
      button:
        text: Commandez votre simulation
        url: /fr/contact/
    design:
      card:
        css_class: "bg-primary-700"
      spacing:
        padding: ["1rem", 0, 0, 0]
---