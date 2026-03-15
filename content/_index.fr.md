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
          position: repeat
          size: auto
          filters:
            brightness: 0.5
      spacing: 
        padding: [0, 0, 0, 0]
        margin: [0, 0, 0, 0]
  - block: resume-biography-3
    id: about
    design:
      spacing: 
        padding: [0, 0, 0, 0]
        margin: [0, 0, 0, 0]
      name:
        size: sm
    content:
      username: admin
      headings:
        about: Qui sommes nous?
      text: |
        Je m'appelle Baptiste Crépin, directeur et expert sécurité chez CravateRouge Ltd.

        Mon aventure en cybersécurité a débutée avec un Master de l'Université Paris-Saclay. Par la suite, j'ai acquis de l'expérience en tant qu'opérateur red team chez AXA, en me spécialisant sur les environments Microsoft. Ce poste m'a permis de simuler des attaques réelles pour identifier et corriger les vulnérabilités.

        Je partage aussi mes recherches et développe des outils afin d'aider les experts en cybersécurité à vous fournir la meilleure protection. Par ailleurs, j'ai eu la chance de présenter l'un de mes outils lors de la [Black Hat Vegas](https://www.blackhat.com/us-22/arsenal/schedule/#bloodyad-26883), l'un des plus grands événements de sécurité informatique au monde.

        C'est pourquoi j'ai décidé de fonder CravateRouge Ltd, afin de mettre mon expertise directement à votre service. Avec des offres de cybersécurité personnalisés et complètes, spécialisées dans les produits Microsoft, nous veillons à ce que votre infrastrucure bénéficie de la meilleure protection. Faites confiance à notre équipe pour sécuriser votre avenir.
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
            Experts sécurité <a href='https://github.com/CravateRouge/bloodyAD'>apprécient notre travail</a>
    spacing:
      padding: ["1rem", 0, "1rem", 0]
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
            <a href='#auditsum'>Testez la résistance de votre infrastructure face aux attaques!</a>
        - name: Protection
          icon: lock-closed
          description: |
            <a href='#protectsum'>Renforcez vos systèmes en implémentant des mesures adaptées</a>
        - name: Formation
          icon: academic-cap
          description: |
            <a href='#trainingsum'>Devenez un As de la cybersécurité!</a>
        - name: Simulation de Phishing
          icon: envelope
          description: |
            <a href='#phishingsum'>Sensiblisez vos employés à identifier les menaces cyber grâce à nos simulations de phishing</a>
        - name: Intégration Microsoft 365
          icon: cpu-chip
          description: |
            <a href='#m365sum'>Intégrez M365 et Azure Cloud à votre entreprise pour améliorer votre productivité et votre sécurité</a>
        - name: Services sur mesure
          icon: beaker
          description: |
            <a href='#tailoredsum'>Si vous avez des besoins spécifiques faites-le nous savoir</a>
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"
      spacing:
        padding: [0, 0, 0, 0]

  - block: cta-image-paragraph
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
            text: Choisissez votre audit
            url: services/audit
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
            url: services/phishing
          id: phishingsum
        - title: Intégration Microsoft 365 & Azure
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
            url: services/integration
          id: m365sum
        - title: Services sur mesure
          text: Un besoin cyber spécifique?
          feature_icon: beaker
          image: craft.png
          button:
            text: Faites nous part de votre projet
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
      spacing:
        padding: ["1rem", 0, 0, 0]
---
