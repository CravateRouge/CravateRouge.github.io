---
title: 'Accueil'
type: landing

design:
  # Default section spacing
  spacing: "0rem"

sections:
  - block: hero
    content:
      title: Renouez avec la sécurité
      text: 🌎 Solutions disponibles à l'international 🌎
      primary_action:
        text: Contactez-nous
        url: contact/
        icon: rocket-launch
      secondary_action:
        text: Découvrez nos services
        url: /#services
      announcement:
        text: "Lire nos derniers"
        link:
          text: "articles"
          url: "/articles/"
    design:
      css_class: "dark"
      background:
        color: "navy"
        image:
          # Add your image background to `assets/media/`.
          filename: constellation.svg
          size: "auto;background-repeat:repeat"
          filters:
            brightness: 0.5
  - block: resume-biography-3
    id: about
    content:
      username: admin

  - block: stats
    content:
      items:
        - statistic: "100+"
          description: |
            Missions réalisées
        - statistic: "6Ans+"
          description: |
            D'expérience sur le terrain
        - statistic: "1500+"
          description: |
            Experts sécurité [apprécient notre travail](https://github.com/CravateRouge/bloodyAD)
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"

  - block: collection
    content:
      title: On parle de nous
      text: Découvrez ce que l'industrie dit de nous
      filters:
        folders:
          - reference
    design:
      view: article-grid
      fill_image: false
      columns: 2
      spacing:
        padding: ["1rem", 0, 0, 0]

  - block: features
    id: services
    content:
      title: Nos Services
      text: 🧱 Construisez une infrastructure robuste 🧱
      items:
        - name: Audit
          icon: bolt
          description: |
            [Testez la résistance de votre infrastructure face aux attaques!](#auditsum)
        - name: Protection
          icon: lock-closed
          description: |
            [Renforcez vos systèmes en implémentant des mesures adaptées](#protectsum)
        - name: Formation
          icon: academic-cap
          description: |
            [Devenez un As de la cybersécurité!](#trainingsum)
        - name: Simulation de Phishing
          icon: envelope
          description: |
            [Sensiblisez vos employés à identifier les menaces cyber grâce à nos simulations de phishing](#phishingsum)
        - name: Intégration Microsoft 365
          icon: cpu-chip
          description: |
            [Intégrez M365 et Azure Cloud à votre entreprise pour améliorer votre productivité et votre sécurité](#m365sum)
        - name: Services sur mesure
          icon: beaker
          description: |
            [Si vous avez des besoins spécifiques faites-le nous savoir](#tailoredsum)
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"

  - block: cta-image-paragraph-custom
    design:
      css_class_primary: "bg-gray-100 dark:bg-gray-900"
    content:
      items:
        - title: Auditez vos systèmes
          text: Testez la résistance de votre infrastructure face aux attaques!
          feature_icon: bolt
          features:
            - "Identifiez les faiblesses de vos applications via des pentests en utilisant des outils automatisés et/ou des attaques avancées en fonction de vos besoins"
            - "Vérifiez votre robustesse face aux cybercriminels en réalisant des simulations d'attaques (red team, menace interne...)"
          image: audit.png
          button:
            text: Commandez un audit
            url: contact/
          id: auditsum
        - title: Protégez votre infrastructure
          text: Renforcez vos systèmes en implémentant des mesures adaptées
          feature_icon: lock-closed
          features:
            - "Mettez vos données en sécurité"
            - "Solutions adaptées à votre entreprise"
            - "Implémentez les dernières normes de sécurité"
          image: protect.png
          button:
            text: Demandez un devis
            url: contact/
          id: protectsum
        - title: Formez vos équipes
          text: Devenez un As de la cybersécurité!
          feature_icon: academic-cap
          features:
            - "Apprenez à développer de manière sécurisée"
            - "Formations sur des sujet cyber spécifiques à vos besoins"
          image: training.png
          button:
            text: Réservez maintenant
            url: contact/
          id: trainingsum
        - title: Simulez une attaque de phishing
          text: Sensibilisez vos équipes aux attaques cyber
          feature_icon: envelope
          features:
            - "Simulez les cyberattaques du monde réel"
            - "Sensibilisez vos employés aux méthodes de phishing les plus courantes"
            - "Personalisez vos scénarios de phishing"
          image: phishing.png
          button:
            text: En savoir plus
            url: contact/
        - title: Intégration Microsoft 365
          text: Un tout-en-un pour améliorer votre productivité et sécurité
          feature_icon: cpu-chip
          features:
            - "Accès unifiés et renforcés"
            - "Gestion du parc informatique centralisée"
            - "Reprenez le contrôle de vos données (conformité RGPD)"
            - "Basé sur les technologies Microsoft™ (Aucun logiciel tiers à installer)"
          image: secureWorkspace.png
          button:
            text: En savoir plus
            url: articles/secure-workspace/
          id: m365sum
        - title: Services sur mesure
          text: Un besoin cyber spécifique?
          feature_icon: beaker
          features:
            - Faites nous part de votre projet
          image: craft.png
          button:
            text: Demandez un devis
            url: contact/
          id: tailoredsum

  - block: cta-card
    content:
      title: Renforcez votre infrastructure
      text: Notre équipe s'en occupe!
      button:
        text: Contactez-nous
        url: contact/
    design:
      card:
        css_class: "bg-primary-700"
      spacing:
        padding: ["1rem", 0, 0, 0]
---
