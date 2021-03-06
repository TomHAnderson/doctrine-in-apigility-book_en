HATEOAS and Hypertext Application Language
==========================================

**Hypertext As The Engine Of Application State**.  This is the lofty goal an API developer should aspire to.  By embedding URLs in your
response a client application doesn't need to know details about the API such as how to paginate or where to find a referenced
resource.  By default Doctrine in Apigility creates canonical self referential links for every entity in a response.  This is a big step
for an API and you get it for free with Doctrine in Apigility.

Hypertext Application Language (HAL) is the dialect of JSON which Apigility speaks.  The notable properties of HAL are self referential
``_links`` and an ``_embedded`` sections in each entity response (``_embedded`` included only when an entity has referenced data).
The ``_links`` array can be modified by the
programmer as the resource is composed thereby allowing custom links to be included with the response.  For instance a link to the
audit trail for a resource may be included along with the canonical self referential link.


Adding Additional Links
-----------------------

Links may be used for anything to link to anywhere.  Some HATEOAS tutorials suggest using links to show other actions such as a POST.
To add more links to an entity as it is rendered use the ``renderEntity`` event in your Module.php file

.. code-block:: php

  <?php
    use Zend\EventManager\Event;
    use Zend\EventManager\EventInterface;
    use ZF\Hal\Link\Link;

    public function onBootstrap(EventInterface $e)
    {
        $app = $e->getTarget();
        $services = $app->getServiceManager();
        $this->container = $services;
        $sharedEvents = $services->get('SharedEventManager');
        $sharedEvents->attach('ZF\Hal\Plugin\Hal', 'renderEntity', array($this, 'onRenderEntity'));
    }

    public function onRenderEntity(Event $e)
    {
        $entity = $e->getParam('entity');

        switch (get_class($entity->getEntity())) {
            case 'Db\Entity\Artist':
                $link = new Link('home');
                $link->setUrl('https://apiskeletons.com');
                $entity->getLinks()->add($link);

                break;
            default:
                break;
        }
    }


Computed Data
-------------

Often it's useful to include computed data with an entity response.  You can do this by attaching to the ``renderEntity.post`` event
in your Module.php file

.. code-block:: php

  <?php
    use Zend\EventManager\Event;
    use Zend\EventManager\EventInterface;

    public function onBootstrap(EventInterface $e)
    {
        $app = $e->getTarget();
        $this->container = $app->getServiceManager();
        $sharedEvents = $this->container->get('SharedEventManager');
        $sharedEvents->attach('ZF\Hal\Plugin\Hal', 'renderEntity.post', array($this, 'onRenderEntityPost'));
    }

    public function onRenderEntityPost(Event $e)
    {
        $objectManager = $this->container->get('doctrine.entitymanager.orm_default');
        $entity = $e->getParam('entity');

        switch (get_class($entity->getEntity())) {
            case 'Db\Entity\Artist':
                $queryBuilder = $objectManager->createQueryBuilder();
                $queryBuilder->select('count(p)')
                    ->from('Db\Entity\Performance', 'p')
                    ->innerJoin('p.artist', 'a')
                    ->andWhere('a.id = :id')
                    ->setParameter('id', $entity->getEntity()->getId())
                ;
                $payload = $e->getParam('payload');
                $payload['_computed'] = array(
                    'performance' => array(
                        'count' => $queryBuilder->getQuery()->getSingleScalarResult(),
                    ),
                );
                break;
            default:
                break;
        }
    }


.. role:: raw-html(raw)
   :format: html

.. note::
  Authored by `API Skeletons <https://apiskeletons.com>`_.  All rights reserved.


:raw-html:`<script async src="https://www.googletagmanager.com/gtag/js?id=UA-64198835-2"></script><script>window.dataLayer = window.dataLayer || [];function gtag(){dataLayer.push(arguments);}gtag('js', new Date());gtag('config', 'UA-64198835-2');</script>`
