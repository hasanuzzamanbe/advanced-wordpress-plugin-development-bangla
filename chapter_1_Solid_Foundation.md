---
title: বেসিক (A Solid Foundation)
parent: Chapters
nav_order: 1
---

# ১. মজবুত ভিত্তি স্থাপন (A Solid Foundation)


ওয়ার্ডপ্রেস প্লাগিন ডেভেলপমেন্টের জগতে আপনাকে স্বাগতম। আপনি যদি এই বইটি হাতে নিয়ে থাকেন, তার মানে আপনি ইতোমধ্যে ওয়ার্ডপ্রেসের বেসিক প্লাগিন তৈরি করতে জানেন। আপনি add\_action, add\_filter ব্যবহার করে ফাংশন হুক করতে পারেন এবং হয়তো কাস্টম পোস্ট টাইপ বা শর্টকোডও তৈরি করেছেন।

কিন্তু যখন একটি প্লাগিন ছোট থেকে মাঝারি বা বড় আকারের হতে শুরু করে, তখন শুধুমাত্র ফাংশন-ভিত্তিক কোড লেখা আর যথেষ্ট থাকে না। কোড অগোছালো হয়ে পড়ে, রক্ষণাবেক্ষণ করা কঠিন হয়ে যায় এবং অন্য কোনো প্লাগিনের সাথে কনফ্লিক্ট করার সম্ভাবনা বেড়ে যায়।

এই অধ্যায়ে আমরা শিখব কীভাবে একটি পেশাদার, রক্ষণাবেক্ষণযোগ্য (maintainable) এবং মাপযোগ্য (scalable) প্লাগিনের জন্য একটি শক্তিশালী ভিত্তি তৈরি করতে হয়। আমরা সাধারণ ফাংশন-ভিত্তিক কোডিং থেকে সরে এসে আধুনিক PHP এবং অবজেক্ট-ওরিয়েন্টেড প্রোগ্রামিং (OOP) এর জগতে প্রবেশ করব।

#### **১.১ কেন সাধারণ ফাংশন-ভিত্তিক প্লাগিন যথেষ্ট নয়?**

ওয়ার্ডপ্রেসের শুরুর দিকে প্রায় সমস্ত প্লাগিনই ছিল ফাংশন-ভিত্তিক। একটি functions.php ফাইলের মতোই, প্লাগিনের মূল ফাইলে অনেকগুলো ফাংশন লেখা হতো। এই পদ্ধতির কিছু বড় দুর্বলতা রয়েছে:

*   নেমস্পেস কনফ্লিক্ট (Namespace Conflict): ওয়ার্ডপ্রেসের ইকোসিস্টেমে হাজার হাজার প্লাগিন এবং থিম রয়েছে। যদি আপনার প্লাগিনের একটি ফাংশনের নাম get\_user\_data() হয়, এবং অন্য আরেকটি জনপ্রিয় প্লাগিনেও একই নামের ফাংশন থাকে, তখন PHP একটি ফ্যাটাল এরর (Fatal Error) দেবে। এর কোনো সহজ সমাধান নেই।

*   গ্লোবাল স্কোপ (Global Scope): সমস্ত ফাংশন এবং ভ্যারিয়েবল গ্লোবাল স্কোপে থাকে, যা কোডকে অগোছালো করে ফেলে। কোন ফাংশন কোথায় কল হচ্ছে বা কোন ভ্যারিয়েবল কোথায় পরিবর্তিত হচ্ছে, তা ট্র্যাক করা কঠিন হয়ে যায়।

*   রক্ষণাবেক্ষণের অসুবিধা: যখন আপনার প্লাগিনে শত শত ফাংশন থাকবে, তখন নতুন ফিচার যুক্ত করা বা কোনো বাগ খুঁজে বের করা অত্যন্ত সময়সাপেক্ষ এবং জটিল হয়ে ওঠে। কোড পুনরায় ব্যবহার (reusability) করাও কঠিন হয়।


এই সমস্যাগুলো সমাধানের জন্যই আমরা আধুনিক PHP ফিচার এবং OOP ব্যবহার করব।

#### **১.২ আধুনিক PHP: Namespace এবং Autoloading**

পেশাদার প্লাগিন তৈরির প্রথম ধাপ হলো আপনার কোডকে একটি স্বতন্ত্র গণ্ডির মধ্যে রাখা, যাতে এটি অন্য কোনো কোডের সাথে সংঘর্ষে না জড়ায়। এর সেরা উপায় হলো Namespace ব্যবহার করা।

Namespace কী?

সহজ ভাষায়, Namespace হলো আপনার কোডের জন্য একটি ভার্চুয়াল ফোল্ডার। এটি আপনার সমস্ত ক্লাস, ফাংশন এবং কনস্ট্যান্টকে একটি অনন্য নামে প্যাকেজ করে রাখে।

উদাহরণস্বরূপ, ধরা যাক আমাদের প্লাগিনের নাম "Awesome Events Manager"। আমরা Awesome\_Events\_Manager বা সংক্ষেপে AEM নামে একটি Namespace তৈরি করতে পারি।

এখানে namespace AEM\\Admin; ঘোষণা করার ফলে Menu ক্লাসটি এখন AEM\\Admin\\Menu নামে পরিচিত হবে। অন্য কোনো প্লাগিনের Menu ক্লাসের সাথে এর আর কোনোদিন কনফ্লিক্ট হবে না।

Autoloading এবং Composer

এখন আপনি Namespace ব্যবহার করে আপনার কোডকে বিভিন্ন ফাইলে এবং ফোল্ডারে সাজিয়ে রাখতে পারবেন। কিন্তু প্রতিটি ফাইলকে require\_once দিয়ে ম্যানুয়ালি লোড করা একটি বিরক্তিকর এবং ত্রুটিপূর্ণ কাজ।

এই সমস্যার সমাধান হলো Autoloader। যখনই আপনার কোডে কোনো ক্লাস ব্যবহার করার প্রয়োজন হবে, অটোলডার স্বয়ংক্রিয়ভাবে সেই ক্লাসের ফাইলটি খুঁজে বের করে লোড করে নেবে। PHP-এর জগতে এই কাজটি করার জন্য স্ট্যান্ডার্ড টুল হলো Composer।

আসুন আমাদের প্লাগিনে Composer সেটআপ করি।

১. আপনার প্লাগিনের রুট ফোল্ডারে টার্মিনাল খুলুন এবং composer init কমান্ডটি চালান। এটি আপনাকে কিছু প্রশ্ন করবে এবং একটি composer.json ফাইল তৈরি করবে।

২. composer.json ফাইলটি খুলুন এবং autoload সেকশনটি যোগ করুন। আমরা এখানে PSR-4 স্ট্যান্ডার্ড ব্যবহার করব, যা Namespace-কে ফাইল পাথের সাথে ম্যাপ করে।

```JSON
{
    "name": "your-vendor/awesome-events-manager",
    "description": "A plugin to manage events.",
    "type": "wordpress-plugin",
    "authors": [
        {
            "name": "Your Name",
            "email": "you@example.com"
        }
    ],
    "require": {},
    "autoload": {
        "psr-4": {
            "AEM\\": "includes/"
        }
    }
}

```

এই কনফিগারেশন Composer-কে বলছে যে AEM\\ Namespace দিয়ে শুরু হওয়া যেকোনো ক্লাসের ফাইল includes/ ফোল্ডারের ভেতরে পাওয়া যাবে। যেমন:

*   AEM\\Admin\\Menu ক্লাসটি থাকবে includes/Admin/Menu.php ফাইলে।

*   AEM\\Frontend\\Shortcode ক্লাসটি থাকবে includes/Frontend/Shortcode.php ফাইলে।


৩. এখন টার্মিনালে composer install বা composer dump-autoload কমান্ডটি চালান। এটি vendor নামে একটি ফোল্ডার তৈরি করবে যার ভেতরে অটোলডার ফাইল থাকবে।

৪. সবশেষে, আপনার প্লাগিনের মূল ফাইলে (যেমন: awesome-events-manager.php) শুধুমাত্র একটি লাইন যোগ করতে হবে:

```PHP
<?php
/**
 * Plugin Name: Awesome Events Manager
 * Description: A plugin to manage events.
 * Version: 1.0.0
 * Author: Your Name
 */

// If this file is called directly, abort.
if ( ! defined( 'WPINC' ) ) {
    die;
}

// Require the Composer autoloader.
require_once __DIR__ . '/vendor/autoload.php';

// Instantiate the main plugin class to get everything started.
// (We will create this class in the next section)
```

ব্যাস! এখন থেকে আপনাকে আর কখনও require বা include নিয়ে চিন্তা করতে হবে না।

#### **১.৩ অবজেক্ট-ওরিয়েন্টেড প্রোগ্রামিং (OOP) এর সঠিক প্রয়োগ**

এখন যেহেতু আমাদের Namespace এবং Autoloader প্রস্তুত, আমরা আমাদের প্লাগিনের আর্কিটেকচার নিয়ে ভাবতে পারি। আমরা একটি মূল ক্লাস (Main Class) তৈরি করব যা পুরো প্লাগিনটিকে চালু এবং নিয়ন্ত্রণ করবে।

ফাইল স্ট্রাকচার:

/awesome-events-manager

|-- awesome-events-manager.php  (মূল প্লাগিন ফাইল)

|-- composer.json

|-- /vendor

|   |-- ... (Composer files)

|-- /includes

    |-- Main.php          (প্লাগিনের মূল কন্ট্রোলার)

    |-- Activator.php     (অ্যাক্টিভেশনের সময় যা ঘটবে)

    |-- Deactivator.php   (ডিঅ্যাক্টিভেশনের সময় যা ঘটবে)

    |-- /Admin

    |   |-- Menu.php      (অ্যাডমিন মেনু তৈরির ক্লাস)

    |-- /Frontend

        |-- Shortcode.php (শর্টকোড তৈরির ক্লাস)

মূল প্লাগিন ফাইল (awesome-events-manager.php)

এই ফাইলটি এখন খুব সাধারণ থাকবে। এর প্রধান কাজ হলো Composer Autoloader-কে লোড করা এবং আমাদের মূল ক্লাসটিকে ইনস্ট্যানশিয়েট (instantiate) করা।

```PHP
<?php
// ... (Plugin header) ...

// If this file is called directly, abort.
if ( ! defined( 'WPINC' ) ) {
    die;
}

// Require the Composer autoloader.
require_once __DIR__ . '/vendor/autoload.php';

/**
 * The main function to run the plugin.
 */
function aem_run_plugin() {
    $plugin = new \AEM\Main();
    $plugin->run();
}

aem_run_plugin();
```

মূল ক্লাস (includes/Main.php)

এই ক্লাসটি হলো আমাদের প্লাগিনের "অর্কেস্ট্রেটর"। এটি অন্যান্য সমস্ত ক্লাসকে শুরু করার এবং হুকগুলোকে রেজিস্টার করার দায়িত্বে থাকবে।

```PHP
<?php
// File: includes/Main.php

namespace AEM;

final class Main {
    /**
     * Run the plugin.
     * Hooks and other actions are initialized here.
     */
    public function run() {
        $this->load_dependencies();
        $this->add_hooks();
    }

    /**
     * Load necessary dependency classes.
     */
    private function load_dependencies() {
        // Here we can instantiate other classes
        // For example: $admin_menu = new \AEM\Admin\Menu();
    }

    /**
     * Add all of the hooks (actions and filters) for the plugin.
     */
    private function add_hooks() {
        // Initialize admin-side functionality
        if (is_admin()) {
            $admin_menu = new \AEM\Admin\Menu();
        }

        // Initialize front-end functionality
        if (!is_admin()) {
            // $shortcode = new \AEM\Frontend\Shortcode();
        }

        // Activation and deactivation hooks
        register_activation_hook(__FILE__, ['AEM\Activator', 'activate']);
        register_deactivation_hook(__FILE__, ['AEM\Deactivator', 'deactivate']);
    }
}

```

দেখুন, আমাদের কোড এখন কতটা পরিষ্কার এবং সুসংগঠিত।

*   প্রতিটি ক্লাসের একটি নির্দিষ্ট দায়িত্ব আছে (Single Responsibility Principle)। Menu ক্লাস শুধু মেনু নিয়ে কাজ করে, Shortcode ক্লাস শুধু শর্টকোড নিয়ে কাজ করে।

*   Main ক্লাসটি সবকিছু শুরু করার দায়িত্ব পালন করে।

*   কোড পড়া এবং বোঝা এখন অনেক সহজ। ৬ মাস পরেও আপনি আপনার নিজের কোড দেখে সহজেই বুঝতে পারবেন।


#### সারসংক্ষেপ

এই অধ্যায়ে আমরা একটি অ্যাডভান্সড প্লাগিনের ভিত্তি স্থাপন করেছি। আমরা শিখেছি:

*   কেন সাধারণ ফাংশন-ভিত্তিক কোডিং বড় প্রজেক্টের জন্য উপযুক্ত নয়।

*   Namespace ব্যবহার করে কীভাবে কোড কনফ্লিক্ট এড়ানো যায়।

*   Composer এবং PSR-4 Autoloader ব্যবহার করে কীভাবে ফাইল লোডিং প্রক্রিয়াকে স্বয়ংক্রিয় করা যায়।

*   OOP ব্যবহার করে কীভাবে একটি পরিচ্ছন্ন, রক্ষণাবেক্ষণযোগ্য এবং মাপযোগ্য প্লাগিন আর্কিটেকচার তৈরি করা যায়।


এই ভিত্তি স্থাপন করা হয়তো শুরুতে একটু বেশি কাজ মনে হতে পারে, কিন্তু আপনার প্লাগিন যখন বড় হতে থাকবে, তখন এই প্রচেষ্টা আপনার বহু ঘন্টা সময় বাঁচিয়ে দেবে এবং আপনাকে একজন দক্ষ ও পেশাদার ডেভেলপার হিসেবে গড়ে তুলবে।

পরবর্তী অধ্যায়ে...

আমরা ওয়ার্ডপ্রেসের অন্যতম শক্তিশালী ফিচার—হুকস (Hooks)—এর গভীরে যাব এবং দেখব কীভাবে অ্যাকশন এবং ফিল্টারকে আরও কার্যকরভাবে ব্যবহার করা যায় ও নিজেদের কাস্টম হুক তৈরি করা যায়।