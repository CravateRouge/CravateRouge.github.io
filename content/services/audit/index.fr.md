---
title: 'Audit'
type: landing

design:
  spacing: "0rem"

sections:
  - block: hero-rel
    content:
      title: Nos Services de Test d'Intrusion
      text: Evaluez la sécurité de votre infrastructure
      secondary_action:
        text: Comprendre la méthodologie
        url: "#methodology"
      announcement:
        text: "Découvrez nos différents types de"
        link:
          text: "test d'intrusion"
          url: "#services"
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

  - block: features
    id: services
    content:
      title: Quel périmètre pour votre test d’intrusion ?
      text: Testez tout ou partie de votre organisation
      items:
        - name: Externe
          icon: share
          description: |
            Testez la vulnérabilité de tout ou partie de votre système d'information exposé, susceptible d'être exploité par des hackers ou des logiciels malveillants.
        - name: Interne
          icon: building-office-2
          description: |
            Testez la vulnérabilité de votre entreprise face à un attaquant interne disposant de certains accès (Accès VPN, compromission via du phishing, accès wifi, etc...). Parfait pour tester la sécurité de votre Active Directory, Réseau et WIFI.
        - name: Web/Applicatif
          icon: globe-alt
          description: |
            Evaluez la sécurité de vos applications et sites web en détectant les failles établies par l'OWASP.
        - name: Cloud
          icon: cloud
          description: |
            Evaluez la sécurité de votre hébergeur Cloud Privé ou Cloud Public (Azure, O365, AWS, GCP).
        - name: Red Team
          icon: user
          description: |
            Simulez une attaque réaliste pour tester la résistance de votre organisation face à des scénarios d'intrusion avancés.
        - name: Purple Team
          icon: users
          description: |
            Combinez les forces de l'attaque et de la défense en travaillant en collaboration avec le SOC (Security Operations Center) pour améliorer votre détection et votre réponse aux menaces.
        - name: Mobile
          icon: device-phone-mobile
          description: |
            Évaluez la sécurité de vos applications Android et iOS en identifiant les vulnérabilités et en protégeant les données sensibles.
        - name: AV/EDR
          icon: eye
          description: |
            Analyse de la couverture et configuration de l'EDR/Antivirus déployé sur votre parc, pour améliorer son efficacité dans la détection d'attaques.
        - name: Intrusion Physique
          icon: wrench-screwdriver
          description: |
            Découvrez les faiblesses dans la sécurité physique des locaux de votre entreprise, comme des portes non verrouillées, des systèmes de surveillance insuffisants, ou des procédures d'accès inappropriées. 
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"

  - block: cta-image-paragraph-custom
    id: methodology
    design:
      css_class_primary: "bg-gray-100 dark:bg-gray-900"
    content:
      title: La méthodologie d'un test d'intrusion
      text: Choisissons la couleur adaptée à vos besoins et vos contraintes.
      items:
        - title: Black Box
          image: black-box.svg
          text: |
            Le test en boite noire consiste à effectuer un pentest sans aucune connaissance préalable de l'environnement cible. Cela reproduit une attaque externe réaliste, en testant principalement les surfaces d'attaque exposées au public, comme les applications web ou les systèmes ouverts. Cette approche permet de mesurer la capacité réelle de l'entreprise à résister à un attaquant externe sans privilèges initiaux. Cependant, elle peut manquer de profondeur pour identifier des failles internes complexes, ce qui est compensé par les méthodes white box ou grey box qui explorent les vulnérabilités internes avec plus de précision.
        - title: White Box
          image: white-box.svg
          text: |
            Un audit en boite blanche consiste à réaliser un test d'intrusion en ayant accès à toutes les informations nécessaires sur l'infrastructure cible : code source, architecture réseau, configurations système, etc. Cette méthode permet d'identifier rapidement des vulnérabilités profondes et complexes qui seraient difficiles à détecter autrement. Elle offre une couverture exhaustive des tests, permettant d'optimiser les ressources et le temps. Toutefois, cette approche peut manquer de réalisme dans le cas où un attaquant externe ne disposerait pas de telles informations, c'est là que le grey box ou le black box testing peuvent apporter un complément en simulant des attaques externes plus réalistes
        - title: Grey Box
          image: grey-box.svg
          text: |
            Le test en boite grise nécessite des informations partielles sur votre système, comme des identifiants limités ou des accès restreints. Cette méthode simule une attaque menée par un utilisateur malveillant ayant un accès partiel à votre infrastructure. Elle permet d'évaluer la sécurité interne tout en reproduisant des scénarios réalistes. C'est un compromis entre les approches black box et white box pour une analyse équilibrée.

  - block: cta-card
    content: 
      title: Trouvez le type de test d'intrusion adapté à vos besoins
      text: |
        Alors que les cyberattaques continuent de menacer la stabilité des entreprises dans tous les secteurs, il est essentiel de trouver le bon prestataire pour effectuer une évaluation précise de votre sécurité. Nous comprenons que chaque organisation a des objectifs et des défis de sécurité uniques, et nous nous efforçons d'adapter nos services pour répondre à vos besoins.
      button:
        text: Demandez un devis
        url: /fr/contact/
    design:
      card:
        css_class: "bg-primary-700"
      spacing:
        padding: ["1rem", 0, 0, 0]
---