---
title: পরিশিষ্ট (Appendix)
parent: Chapters
nav_order: 11
---


# ১১: পরিশিষ্ট (Appendix)

এই অংশটি আপনার রেফারেন্স হিসেবে কাজ করবে। এখানে কিছু গুরুত্বপূর্ণ হুক, ফিল্টার, কোড স্নিপেট এবং অনলাইন রিসোর্সের একটি তালিকা দেওয়া হলো যা আপনার প্লাগিন ডেভেলপমেন্টের যাত্রায় সবসময় কাজে লাগবে।


#### গুরুত্বপূর্ণ হুক এবং ফিল্টারের তালিকা

এখানে এমন কিছু বহুল ব্যবহৃত অ্যাকশন এবং ফিল্টার হুকের তালিকা দেওয়া হলো যা প্রায় প্রতিটি প্লাগিনেই কোনো না কোনো সময় প্রয়োজন হয়।

অ্যাক্টিভেশন / ডিঅ্যাক্টিভেশন হুক:

-   register_activation_hook(): প্লাগিন অ্যাক্টিভেট হওয়ার সময় একবার রান হয়। কাস্টম টেবিল তৈরি বা ডিফল্ট অপশন সেট করার জন্য ব্যবহৃত হয়।

-   register_deactivation_hook(): প্লাগিন ডিঅ্যাক্টিভেট হওয়ার সময় একবার রান হয়। সাধারণত কোনো কিছু পরিষ্কার (cleanup) করার কাজে ব্যবহৃত হয়।

সাধারণ হুক (General Hooks):

-   plugins_loaded: সব প্লাগিন লোড হওয়ার পর এই অ্যাকশনটি ফায়ার হয়। টেক্সট ডোমেইন লোড করার জন্য এটিই সেরা জায়গা।

-   init: ওয়ার্ডপ্রেসের প্রায় সবকিছু (যেমন: পোস্ট টাইপ, ট্যাক্সনমি, ইউজার ডেটা) লোড হওয়ার পর এই অ্যাকশনটি ফায়ার হয়। কাস্টম পোস্ট টাইপ বা ট্যাক্সনমি রেজিস্টার করার জন্য এটি উপযুক্ত।

-   wp_enqueue_scripts: ফ্রন্টএন্ডে CSS এবং JavaScript ফাইল এনকিউ (enqueue) করার জন্য এই অ্যাকশনটি ব্যবহৃত হয়।

-   admin_enqueue_scripts: অ্যাডমিন ড্যাশবোর্ডে CSS এবং JavaScript ফাইল এনকিউ করার জন্য এই অ্যাকশনটি ব্যবহৃত হয়।

পোস্ট এবং কনটেন্ট সম্পর্কিত ফিল্টার:

-   the_content: পোস্টের মূল কনটেন্ট প্রদর্শনের আগে তা পরিবর্তন করার জন্য এই ফিল্টারটি ব্যবহৃত হয়।

-   the_title: পোস্টের শিরোনাম প্রদর্শনের আগে তা পরিবর্তন করার জন্য এই ফিল্টারটি ব্যবহৃত হয়।

-   save_post: যখন একটি পোস্ট তৈরি বা আপডেট করা হয়, তখন এই অ্যাকশনটি ফায়ার হয়।

অ্যাডমিন সম্পর্কিত হুক:

-   admin_menu: অ্যাডমিন মেনুতে নতুন পেজ যোগ করার জন্য এই অ্যাকশনটি ব্যবহৃত হয়।

-   admin_init: অ্যাডমিন স্ক্রিন লোড হওয়ার শুরুতে এই অ্যাকশনটি ফায়ার হয়। সেটিংস API রেজিস্টার করার জন্য এটি উপযুক্ত।

* * * * *

#### প্রয়োজনীয় কোড স্নিপেট

এখানে কিছু সাধারণ কিন্তু প্রয়োজনীয় কোড স্নিপেট দেওয়া হলো যা আপনি সরাসরি আপনার প্রজেক্টে ব্যবহার করতে পারবেন।

১. কাস্টম পোস্ট টাইপ রেজিস্টার করা:

```PHP

function  aem_register_event_post_type() {
    $labels = [
 'name' => _x('Events', 'post type general name', 'textdomain'),
 'singular_name'      => _x('Event', 'post type singular name', 'textdomain'),
 'add_new'            => __('Add New', 'textdomain'),
 'add_new_item' => __('Add New Event', 'textdomain'),
 'edit_item'          => __('Edit Event', 'textdomain'),
 'new_item' => __('New Event', 'textdomain'),
 'all_items'          => __('All Events', 'textdomain'),
 'view_item'          => __('View Event', 'textdomain'),
 'search_items' => __('Search Events', 'textdomain'),
 'not_found'          => __('No events found', 'textdomain'),
 'not_found_in_trash' => __('No events found in Trash', 'textdomain'),
    ];

    $args = [
 'labels' => $labels,
 'public' => true,
 'has_archive'=> true,
 'supports' => ['title', 'editor', 'thumbnail'],
 'rewrite'            => ['slug' => 'events'],
 'menu_icon'=> 'dashicons-calendar-alt',
    ];

    register_post_type('event', $args);
}
add_action('init', 'aem_register_event_post_type');

```

২. AJAX হ্যান্ডেল করার বেসিক কাঠামো:


```JavaScript
 JavaScript (main.js)
jQuery.ajax({
    url: my_ajax_object.ajax_url,
type: 'POST',
    data: {
action: 'my_custom_action',
        nonce: my_ajax_object.nonce,
 // other data
    },
success: function(response) {
        console.log(response);
    }
});

```

```PHP

// PHP (functions.php or your plugin file)\
add_action('wp_ajax_my_custom_action', 'my_custom_action_callback'); // For logged-in users\
add_action('wp_ajax_nopriv_my_custom_action', 'my_custom_action_callback'); // For non-logged-in users

function  my_custom_action_callback() {
    check_ajax_referer('my_ajax_nonce', 'nonce');

 // Process your data here

    wp_send_json_success('Data processed successfully!');
 // or wp_send_json_error('Something went wrong.');

    wp_die();
}

// Localize script to pass PHP variables to JavaScript
wp_localize_script('my-main-script', 'my_ajax_object', [
 'ajax_url' => admin_url('admin-ajax.php'),
 'nonce'    => wp_create_nonce('my_ajax_nonce'),
]);

```


#### সহায়ক রিসোর্স এবং লিঙ্ক

ওয়ার্ডপ্রেস ডেভেলপমেন্ট একটি চলমান শেখার প্রক্রিয়া। নিচে কিছু অপরিহার্য অনলাইন রিসোর্সের লিঙ্ক দেওয়া হলো যা আপনাকে সবসময় আপ-টু-ডেট থাকতে এবং সমস্যার সমাধান খুঁজে পেতে সাহায্য করবে।

-   WordPress Developer Resources:[  developer.wordpress.org](https://developer.wordpress.org/)

-   এটি হলো ওয়ার্ডপ্রেসের অফিসিয়াল ডকুমেন্টেশন। যেকোনো ফাংশন, হুক বা API সম্পর্কে বিস্তারিত জানার জন্য এটিই সেরা জায়গা।

-   WordPress Coding Standards:[  developer.wordpress.org/coding-standards/](https://developer.wordpress.org/coding-standards/)

-   আপনার কোডকে পরিচ্ছন্ন এবং স্ট্যান্ডার্ড মানের রাখার জন্য এই গাইডলাইনগুলো অনুসরণ করা জরুরি।

-   WP-CLI Official Site:[  wp-cli.org](https://wp-cli.org/)

-   WP-CLI-এর সমস্ত কমান্ড এবং তাদের ব্যবহার সম্পর্কে বিস্তারিত ডকুমেন্টেশন।

-   WordPress Stack Exchange:[  wordpress.stackexchange.com](https://wordpress.stackexchange.com/)

-   ওয়ার্ডপ্রেস সম্পর্কিত যেকোনো প্রশ্ন করার এবং অন্যদের প্রশ্নের উত্তর খোঁজার জন্য একটি চমৎকার কমিউনিটি প্ল্যাটফর্ম।

-   Post Status:[  poststatus.com](https://poststatus.com/)

-   ওয়ার্ডপ্রেস কমিউনিটির খবর এবং পেশাদার ডেভেলপারদের জন্য লেখা আর্টিকেলের জন্য একটি জনপ্রিয় ওয়েবসাইট।

-   Tom McFarlin's Blog:[  tommcfarlin.com](https://tommcfarlin.com/)

-   ওয়ার্ডপ্রেস প্লাগিন ডেভেলপমেন্টের সেরা অনুশীলন এবং সফটওয়্যার ইঞ্জিনিয়ারিং নিয়ে লেখা高质量 (high-quality) আর্টিকেলের জন্য পরিচিত।

* * * * *

শেষ কথা (Conclusion)
--------------------

এই বইয়ের যাত্রা এখানেই শেষ হলো। আমরা একটি সাধারণ আইডিয়া থেকে শুরু করে একটি সম্পূর্ণ, নিরাপদ, দ্রুত, বিশ্বমানের এবং আয়-সক্ষম প্লাগিন তৈরির প্রতিটি ধাপ অতিক্রম করেছি। আপনি শুধু নির্দিষ্ট কিছু ফাংশন বা API শেখেননি, বরং একজন পেশাদার ওয়ার্ডপ্রেস ডেভেলপার হিসেবে কীভাবে চিন্তা করতে হয়, কীভাবে একটি প্রোডাক্ট তৈরি করতে হয় এবং কীভাবে সেটিকে বিশ্বের কাছে পৌঁছে দিতে হয়, তা শিখেছেন।

ওয়ার্ডপ্রেস ইকোসিস্টেম বিশাল এবং সম্ভাবনাময়। এখানে শেখার এবং তৈরি করার কোনো শেষ নেই। এই বইটি আপনার যাত্রার একটি শক্তিশালী সূচনা মাত্র। এখন আপনার কাজ হলো এই জ্ঞানকে ব্যবহার করে নতুন কিছু তৈরি করা, সমস্যার সমাধান করা এবং এই অসাধারণ কমিউনিটিতে নিজের অবদান রাখা।

আপনার কোডিং যাত্রা আনন্দময় এবং সফল হোক! শুভকামনা!