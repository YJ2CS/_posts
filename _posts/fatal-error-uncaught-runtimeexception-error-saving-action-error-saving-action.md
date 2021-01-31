---
title: >-
  Fatal error: Uncaught RuntimeException: Error saving action: Error saving
  action
url: fatal-error-uncaught-runtimeexception-error-saving-action-error-saving-action
author: YJ2CS
avatar: /custom/avatar.webp
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 文章
tags:
  - 悦读
no-photos: >-
  https://random.52ecy.cn/randbg.php?size=1&rid-fatal-error-uncaught-runtimeexception-error-saving-action-error-saving-action
date: '2021-01-03T12:34:56+08:00'
date updated: '2021-01-03T14:07:43+08:00'

---

> Core passions and aspirations should be consistent and in sync.
> — <cite>Lorii Myers</cite>

#92438

Hello,
after updating the plugin, I get this error:

```text
Fatal error: Uncaught RuntimeException: Error saving action: Error saving action: Table ‘astrella_itwordpress.www_actionscheduler_actions’ doesn’t exist in /customers/7/2/4/astrella.it/httpd.www/wp-content/plugins/seo-by-rank-math/vendor/woocommerce/action-scheduler/classes/migration/ActionScheduler_DBStoreMigrator.php:44 Stack trace: #0 /customers/7/2/4/astrella.it/httpd.www/wp-content/plugins/seo-by-rank-math/vendor/woocommerce/action-scheduler/classes/data-stores/ActionScheduler_HybridStore.php(242): ActionScheduler_DBStoreMigrator->save_action(Object(ActionScheduler_Action), NULL) #1 /customers/7/2/4/astrella.it/httpd.www/wp-content/plugins/seo-by-rank-math/vendor/woocommerce/action-scheduler/classes/ActionScheduler_ActionFactory.php(177): ActionScheduler_HybridStore->save_action(Object(ActionScheduler_Action)) #2 /customers/7/2/4/astrella.it/httpd.www/wp-content/plugins/seo-by-rank-math/vendor/woocommerce/action-scheduler/classes/ActionScheduler_ActionFactory.php(105): ActionScheduler_ActionFactory->store(Object(Acti in /customers/7/2/4/astrella.it/httpd.www/wp-content/plugins/seo-by-rank-math/vendor/woocommerce/action-scheduler/classes/migration/ActionScheduler_DBStoreMigrator.php on line 44

```

Can you help me please?

Thank you
Massimo

- <https://support.rankmath.com/ticket/fatal-error-uncaught-runtimeexception-error-saving-action-error-saving-action/#post-92462>)

  Hello,

  Sorry for any inconvenience that might have been caused due to that.

  It seems like new tables are not getting created in the Database or some old plugin has left an incorrect entry.

  **Please follow these steps to fix the issue:**

  **1. Take a complete backup of your website**
  **2.** Install this plugin: <https://wordpress.org/plugins/code-snippets/>
  **3.** Head over to the Snippets menu in the left sidebar and click Add New
  add new snippet
  **4.** Give that post any name and copy the below code in the Code field

  ```php
  add_action( 'init', function() {
  	delete_option( 'schema-ActionScheduler_StoreSchema' );
  } );
  ```

  **5.** Select ‘Only run in administration area’ option and click the Save and Activate button
  run only in admin area wordpress
  **6.** Open WordPress Dashboard in a new window
  **7.** Then deactivate recently created snippet from here
  disable snippet
  **8.** Then activate the Rank Math plugin again
  **9.** Please disable the Code Snippets plugin if you are not going to use it

  **Here’s a video screencast:**
  <https://i.rankmath.com/79gPbv>

  Please clear all the cache, including the server after following the above process.

  That should resolve the issue. Thank you.
