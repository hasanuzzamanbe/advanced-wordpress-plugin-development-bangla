---
title: অ্যাডভান্সড API-এর ব্যবহার
parent: Chapters
nav_order: 4
---

# ৪: অ্যাডভান্সড API-এর ব্যবহার (Using Advanced APIs)


ওয়ার্ডপ্রেস শুধু একটি কনটেন্ট ম্যানেজমেন্ট সিস্টেমই নয়, এটি একটি পাওয়ারফুল অ্যাপ্লিকেশন ডেভেলপমেন্ট ফ্রেমওয়ার্কও। এই শক্তির মূল উৎস হলো এর বিশাল API (Application Programming Interface) লাইব্রেরি। API হলো কিছু পূর্ব-লিখিত ফাংশন এবং ক্লাসের সমষ্টি যা ডেভেলপারদেরকে ওয়ার্ডপ্রেসের মূল সিস্টেমের সাথে একটি স্ট্যান্ডার্ড ও সিকিউর উপায়ে যোগাযোগ করতে সাহায্য করে।

যখন আপনি কোর API ব্যবহার করেন, তখন আপনি নিজের সময় বাঁচান, নিরাপত্তা নিশ্চিত করেন এবং আপনার প্লাগিনকে ওয়ার্ডপ্রেসের ভবিষ্যৎ আপডেটের সাথে সামঞ্জস্যপূর্ণ রাখেন। এই অধ্যায়ে আমরা ওয়ার্ডপ্রেসের তিনটি অত্যন্ত গুরুত্বপূর্ণ API নিয়ে আলোচনা করব যা আপনার প্লাগিনকে নেক্সট লেভেলে নিয়ে যাবে: **Settings API**, **REST API**, এবং **Filesystem API**।

### **৪.১ Settings API: পেশাদার অপশন পেজ তৈরি**

প্রায় প্রতিটি প্লাগিনেরই কিছু সেটিংস থাকে যা অ্যাডমিনকে কনফিগার করতে হয়। আপনি হয়তো সাধারণ HTML ফর্ম এবং update\_option() ও get\_option() ব্যবহার করে এটি করতে পারেন, কিন্তু এই পদ্ধতির বেশ কিছু দুর্বলতা আছে:

*   **নিরাপত্তা:** আপনাকে ম্যানুয়ালি Nonce ভেরিফিকেশন করতে হবে CSRF অ্যাটাক থেকে বাঁচার জন্য।

*   **ডেটা ভ্যালিডেশন:** ব্যবহারকারীর ইনপুটকে ডেটাবেসে সেভ করার আগে বিশুদ্ধ (sanitise) করার দায়িত্ব আপনার।

*   **অগোছালো কোড:** একটি ফাইলের মধ্যেই HTML, PHP এবং ডেটাবেস লজিক মিলেমিশে একাকার হয়ে যায়।


এই সব সমস্যার সমাধান হলো **Settings API**। এটি একটি সুসংগঠিত উপায়ে সেটিংস পেজ তৈরি, রেজিস্টার এবং ম্যানেজ করার পুরো প্রক্রিয়াটি সামলায়।

**Settings API-এর মূল উপাদান:**

1.  register\_setting(): এটি আপনার সেটিংস গ্রুপ এবং একটি নির্দিষ্ট অপশন নেম (option\_name) রেজিস্টার করে। এখানেই আপনি একটি স্যানিটাইজেশন কলব্যাক ফাংশন যুক্ত করতে পারেন।

2.  add\_settings\_section(): এটি আপনার সেটিংস পেজে একটি নতুন বিভাগ বা সেকশন তৈরি করে।

3.  add\_settings\_field(): এটি একটি সেকশনের ভেতর একটি নির্দিষ্ট সেটিংস ফিল্ড (যেমন: টেক্সটবক্স, চেকবক্স) যোগ করে এবং সেই ফিল্ডের HTML রেন্ডার করার জন্য একটি কলব্যাক ফাংশন নির্ধারণ করে।


**পূর্ণাঙ্গ উদাহরণ: একটি সেটিংস পেজ তৈরি**

আমাদের "Awesome Events Manager" প্লাগিনের জন্য একটি সেটিংস পেজ তৈরি করা যাক।

```php
<?php
// includes/Admin/SettingsPage.php

namespace AEM\Admin;

class SettingsPage {
    public function __construct() {
        add_action('admin_menu', [$this, 'add_settings_menu']);
        add_action('admin_init', [$this, 'register_settings']);
    }

    public function add_settings_menu() {
        add_options_page(
            'Awesome Events Settings',
            'Awesome Events',
            'manage_options',
            'aem-settings',
            [$this, 'render_settings_page']
        );
    }

    public function register_settings() {
        // ধাপ ১: সেটিংস রেজিস্টার করা
        register_setting(
            'aem_settings_group',         // অপশন গ্রুপ
            'aem_option',                 // ডেটাবেসে এই নামে সেভ হবে
            [$this, 'sanitize_options']   // স্যানিটাইজেশন কলব্যাক
        );

        // ধাপ ২: সেকশন যোগ করা
        add_settings_section(
            'aem_general_section',        // সেকশন আইডি
            'General Settings',           // সেকশন টাইটেল
            [$this, 'render_section_text'], // সেকশনের বিবরণী
            'aem-settings'                // পেজের স্লাগ
        );

        // ধাপ ৩: ফিল্ড যোগ করা
        add_settings_field(
            'aem_api_key_field',          // ফিল্ড আইডি
            'API Key',                    // ফিল্ড লেবেল
            [$this, 'render_api_key_field'],// ফিল্ডের HTML রেন্ডার ফাংশন
            'aem-settings',               // পেজের স্লাগ
            'aem_general_section'         // যে সেকশনে যুক্ত হবে
        );
    }

    public function render_settings_page() {
        ?>
        <div class="wrap">
            <h1>Awesome Events Settings</h1>
            <form action="options.php" method="post">
                <?php
                settings_fields('aem_settings_group');
                // Nonce এবং অন্যান্য হিডেন ফিল্ড যোগ করে
                do_settings_sections('aem-settings');
                // রেজিস্টার করা সব সেকশন ও ফিল্ড দেখায়
                submit_button('Save Settings');
                ?>
            </form>
        </div>
        <?php
    }


public function render_section_text() {

      echo '<p>Configure general settings for the Awesome Events plugin.</p>';

}

public function render_api_key_field() {
        $options = get_option('aem_option');
        $api_key = isset($options['api_key']) ? esc_attr($options['api_key']) : '';
        echo "<input type='text' name='aem_option[api_key]' value='{$api_key}' class='regular-text' />";
    }



    public function sanitize_options($input) {
        $new_input = [];
        if (isset($input['api_key'])) {
            $new_input['api_key'] = sanitize_text_field($input['api_key']);
        }
        return $new_input;
    }
}

```

**৪.২ REST API: ওয়ার্ডপ্রেসকে হেডলেস করা**

REST API হলো একটি আধুনিক ইন্টারফেস যা অ্যাপ্লিকেশনগুলোকে HTTP প্রোটোকল ব্যবহার করে আপনার ওয়ার্ডপ্রেস সাইটের ডেটার সাথে যোগাযোগ করতে দেয়। এর মাধ্যমে আপনি জাভাস্ক্রিপ্ট-ভিত্তিক ফ্রন্টএন্ড (React, Vue), মোবাইল অ্যাপ বা অন্য কোনো সার্ভার থেকে আপনার ওয়ার্ডপ্রেসের ডেটা পড়তে (Read) বা লিখতে (Write) পারবেন।

ওয়ার্ডপ্রেসের নিজস্ব এন্ডপয়েন্ট (যেমন: পোস্ট, পেজ, ইউজার পড়া) রয়েছে। কিন্তু একজন অ্যাডভান্সড ডেভেলপার হিসেবে আপনার শক্তি হলো কাস্টম এন্ডপয়েন্ট তৈরি করায়।

কাস্টম REST API এন্ডপয়েন্ট তৈরি:

1.  rest\_api\_init হুক ব্যবহার করুন।

2.  register\_rest\_route() ফাংশনটি কল করুন।


এই ফাংশনটি তিনটি প্রধান আর্গুমেন্ট নেয়:

*   Namespace: আপনার প্লাগিনের জন্য একটি অনন্য নাম (e.g., aem/v1)।

*   Route: এন্ডপয়েন্টের URL পাথ (e.g., /log/(?P\\d+))। এখানে রেগুলার এক্সপ্রেশন ব্যবহার করে URL থেকে প্যারামিটার ক্যাপচার করা যায়।

*   Arguments Array: একটি অ্যারে যেখানে methods (GET, POST), callback (যখন এন্ডপয়েন্টে হিট করা হয় তখন কোন ফাংশন রান করবে) এবং permission\_callback (নিরাপত্তা: কে এই এন্ডপয়েন্ট অ্যাক্সেস করতে পারবে) ডিফাইন করা হয়।


উদাহরণ: একটি লগ এন্ট্রি পড়ার জন্য GET এন্ডপয়েন্ট

তৃতীয় অধ্যায়ে আমরা যে aem\_event\_logs টেবিল তৈরি করেছিলাম, সেখান থেকে একটি নির্দিষ্ট লগ পড়ার জন্য একটি এন্ডপয়েন্ট তৈরি করা যাক।

```php
<?php
// includes/Api/Routes.php

namespace AEM\Api;

class Routes {
    public function __construct() {
        add_action('rest_api_init', [$this, 'register_routes']);
    }

    public function register_routes() {
        register_rest_route('aem/v1', '/log/(?P<id>\d+)', [
            'methods'  => 'GET', // \WP_REST_Server::READABLE
            'callback' => [$this, 'get_log_entry'],
            'permission_callback' => [$this, 'get_log_permission'],
            'args' => [ // URL প্যারামিটারের বর্ণনা
                'id' => [
                    'validate_callback' => function($param) {
                        return is_numeric($param);
                    },
                    'required' => true,
                ],
            ],
        ]);
    }

    public function get_log_entry(\WP_REST_Request $request) {
        global $wpdb;
        $table_name = $wpdb->prefix . 'aem_event_logs';
        $log_id = $request['id'];

        $log = $wpdb->get_row(
            $wpdb->prepare("SELECT * FROM $table_name WHERE log_id = %d", $log_id)
        );

        if (empty($log)) {
            return new \WP_Error('not_found', 'Log entry not found', ['status' => 404]);
        }

        return new \WP_REST_Response($log, 200);
    }

    public function get_log_permission() {
        // শুধুমাত্র অ্যাডমিনরাই এই এন্ডপয়েন্ট অ্যাক্সেস করতে পারবে
        return current_user_can('manage_options');
    }
}

```

এখন https://yourdomain.com/wp-json/aem/v1/log/123 এই URL-এ GET রিকোয়েস্ট পাঠালে ১২৩ নম্বর লগ এন্ট্রির ডেটা JSON ফরম্যাটে পাওয়া যাবে।

### **৪.৩ Filesystem API: সিকিউরে ফাইল ম্যানেজ করা**

যখন আপনার প্লাগিনের সার্ভারে ফাইল লেখা বা পড়ার প্রয়োজন হয় (যেমন: লগ ফাইল তৈরি, CSS ফাইল জেনারেট করা), তখন PHP-এর সাধারণ file\_put\_contents() বা fopen() ফাংশন ব্যবহার করা ঝুঁকিপূর্ণ। কারণ বিভিন্ন হোস্টিং এনভাইরনমেন্টে ফাইলের পারমিশন ভিন্ন ভিন্ন হতে পারে, যা আপনার কোডকে অবিশ্বস্ত করে তোলে।

Filesystem API এই কাজটি করার জন্য একটি সিকিউর অ্যাবস্ট্র্যাকশন লেয়ার প্রদান করে। এটি সার্ভারের কনফিগারেশন অনুযায়ী সঠিক ফাইল অ্যাক্সেস মেথড (Direct, FTP, SSH) নিজে থেকেই বেছে নেয়।

উদাহরণ: একটি ফাইলে লগ লেখা

```php
function write_to_log_file($message) {
    // WP_Filesystem API লোড করার জন্য
    require_once(ABSPATH . 'wp-admin/includes/file.php');

    global $wp_filesystem;

    // ফাইল সিস্টেম ইনিশিয়ালাইজ করা
    if (WP_Filesystem()) {
        $upload_dir = wp_upload_dir();
        $log_file = $upload_dir['basedir'] . '/aem-logs.txt';

        // বর্তমান কন্টেন্টের সাথে নতুন মেসেজ যোগ করা
        $current_content = '';
        if ($wp_filesystem->exists($log_file)) {
            $current_content = $wp_filesystem->get_contents($log_file);
        }

        $new_content = $current_content . current_time('mysql') . ': ' . $message . "\n";

        // ফাইলে লেখা
        $wp_filesystem->put_contents($log_file, $new_content, FS_CHMOD_FILE);
    }
}

```

#### সারসংক্ষেপ

এই অধ্যায়ে আমরা ওয়ার্ডপ্রেসের তিনটি পাওয়ারফুল API সম্পর্কে শিখেছি যা আপনার প্লাগিনকে আরও প্রফেশনাল করে তুলবে:

*   Settings API ব্যবহার করে আমরা সিকিউর এবং সুসংগঠিত অপশন পেজ তৈরি করতে পারি।

*   REST API দিয়ে আমরা কাস্টম এন্ডপয়েন্ট তৈরি করতে পারি, যা ওয়ার্ডপ্রেসকে একটি হেডলেস CMS হিসেবে ব্যবহার করার দরজা খুলে দেয়।

*   Filesystem API ব্যবহার করে আমরা বিভিন্ন হোস্টিং এনভাইরনমেন্টে সিকিউরে এবং নির্ভরযোগ্যভাবে ফাইল রিড ও রাইট করতে পারি।


এই API-গুলো আয়ত্ত করার মাধ্যমে আপনি শুধু কোডই লিখছেন না, বরং ওয়ার্ডপ্রেস ফ্রেমওয়ার্কের সাথে একীভূত হয়ে পাওয়ারফুল এবং স্থিতিশীল অ্যাপ্লিকেশন তৈরি করছেন।

পরবর্তী অধ্যায়ে...

আমরা এখন ওয়ার্ডপ্রেসের ব্যাকএন্ডের বিভিন্ন পাওয়ারফুল API ব্যবহার করতে শিখেছি। এবার চলুন আধুনিক ফ্রন্টএন্ডের দিকে নজর দেওয়া যাক। পরবর্তী অধ্যায়ে আমরা শিখব কীভাবে গুটেনবার্গ এডিটরের জন্য কাস্টম ব্লক তৈরি করতে হয়, যা আজকের ওয়ার্ডপ্রেস ডেভেলপমেন্টের একটি অপরিহার্য অংশ।