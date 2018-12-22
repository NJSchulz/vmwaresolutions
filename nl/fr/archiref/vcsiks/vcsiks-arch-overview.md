---

copyright:

  years:  2016, 2018

lastupdated: "2018-11-16"

---

# Présentation de l'architecture
Les offres {{site.data.keyword.vmwaresolutions_full}} fournissent l'automatisation du déploiement des composants de technologie VMware dans les {{site.data.keyword.CloudDataCents_notm}} situés dans le monde entier.
L'architecture est composée d'une région de cloud et a la capacité de s'étendre dans d'autres régions de cloud situées dans une autre zone géographique et/ou dans un autre pod {{site.data.keyword.cloud_notm}} au sein du même centre de données.

Vous pouvez déployer manuellement les produits {{site.data.keyword.cloud_notm}} Private (ICP) et Cloud Automation Manager (CAM) dans votre plateforme de virtualisation sur site, permettant ainsi la gestion du cloud à partir des emplacements locaux. Sinon, ICP et CAM sont offerts en tant qu'extension de service à un déploiement VMware vCenter Server on {{site.data.keyword.cloud_notm}} nouveau ou existant, via l'automatisation, permettant ainsi la gestion du cloud à partir d'{{site.data.keyword.cloud_notm}}. 

ICP est une plateforme applicative pour le développement et la gestion sur site d'applications conteneurisées. ICP est un environnement intégré pour la gestion de conteneurs qui inclut l'orchestrateur de conteneurs Kubernetes, un registre d'images privé, une console de gestion, ainsi que des infrastructures préfabriquées de surveillance.

IBM Multi-Cluster Manager fournit la visibilité utilisateur, la gestion orientée applications (règles, déploiements, santé, opérations) et la conformité basée sur les règles sur les clouds et les clusters. IBM Multi-Cluster Manager vous permet de contrôler vos clusters Kubernetes. Vous pouvez vérifier que vos clusters sont sécurisés, qu'ils fonctionnent efficacement et qu'ils distribuent les niveaux de service prévus pour les applications.

{{site.data.keyword.cloud_notm}} Automation Manager est une plateforme de gestion libre-service multi-cloud qui s'exécute sur {{site.data.keyword.cloud_notm}} Private et permet aux développeurs et aux administrateurs de répondre aux besoins de l'entreprise. Cloud Automation Manager Service Composer vous permet d'exposer des services cloud hybrides dans le catalogue IBM Cloud Private.

## Plateforme de gestion du cloud côté IBM Cloud

Le diagramme ci-dessous représente ICP et CAM déployés avec l'infrastructure {{site.data.keyword.cloud_notm}}, avec des connexions aux services vCenter et IBM Kubernetes Service (IKS) locaux déployés sur {{site.data.keyword.cloud_notm}}. Les utilisateurs peuvent déployer des machines virtuelles sur site, des machines virtuelles dans des instances vCenter Server, ainsi que des conteneurs sur le cluster ICP et IKS. 

Figure 1. Gestion du cloud côté cloud
![Gestion du cloud côté cloud](vcsiks-oncloud-cloudmgt.svg)

Dans le diagramme, CAM crée des connexions de cloud aux services vCenter, aux fournisseurs de cloud et aux environnements ICP et IKS de façon logique. Des clusters ICP doivent être déployés dans chaque centre de données ou environnement de cloud, MCM fournissant le mécanisme de connexion aux clusters ICP dans une seule vue de gestion.

ICP peut être déployé avec des composants NSX-V ou NSX-T. ICP avec NSX-V permet d'exécuter les machines virtuelles ICP sur le réseau VXLAN et d'utiliser la mise en réseau interne Kubernetes Calico.

ICP avec NSX-T permet aux utilisateurs de contrôler et configurer la mise en réseau, le sous-réseau et les règles à partir d'une interface utilisateur centralisée (NSX-T Manager). Pour plus d'informations sur les différences entre NSX-V et NSX-T, voir [Architecture de référence de la mise en réseau VCS {{site.data.keyword.cloud_notm}}](../vcsnsxt/vcsnsxt-intro.html).

## Plateforme de gestion du cloud sur site

Le diagramme ci-dessous représente ICP et CAM déployés dans l'infrastructure sur site, avec des connexions aux services vCenter et IKS déployés sur {{site.data.keyword.cloud_notm}}. Les utilisateurs peuvent déployer des machines virtuelles et des conteneurs sur site, des machines virtuelles dans des instances vCenter Server et des conteneurs sur le cluster IKS.

Figure 2. Gestion du cloud sur site
![Gestion du cloud sur site](vcsiks-onprem-cloudmgt.svg)

Le réseau privé virtuel strongSwan est utilisé pour établir une connectivité avec les conteneurs IKS déployés. strongSwan peut par la suite être remplacé par la connectivité Direct-link.


Dans le diagramme, CAM crée des connexions de cloud aux services vCenter, aux fournisseurs de cloud et aux environnements ICP et IKS de façon logique. Des clusters ICP doivent être déployés dans chaque centre de données ou environnement de cloud, MCM fournissant le mécanisme de connexion aux clusters ICP dans une seule vue de gestion.

### Liens connexes

* [Présentation de vCenter Server on {{site.data.keyword.cloud_notm}} with Hybridity Bundle](../vcs/vcs-hybridity-intro.html)