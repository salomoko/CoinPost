Rails 3.1 Integration for TinyMCE
=================================

The `tinymce-rails` gem integrates the [TinyMCE](http://www.tinymce.com/) editor with the Rails 3.1 asset pipeline.


Instructions
------------

**1. Add `tinymce-rails` to your Gemfile**

    gem 'tinymce-rails'

Be sure to add to the global group, not the `assets` group. Then run `bundle install`.


**2. Create a `config/tinymce.yml` file with your global configuration options:**

    theme_advanced_toolbar_location: top
    theme_advanced_toolbar_align: left
    theme_advanced_statusbar_location: bottom
    theme_advanced_buttons3_add:
      - tablecontrols
      - fullscreen
    plugins:
      - table
      - fullscreen

See the [TinyMCE Documentation](http://www.tinymce.com/wiki.php/Configuration) for a full list of configuration options.


**3. Include the TinyMCE assets**

Add to your application.js:

    //= require tinymce

or with jQuery integration:

    //= require tinymce-jquery

The TinyMCE assets can be included on a per-page basis using the `tinymce_assets` helper:

    <%= tinymce_assets %>
    #=> <script type="text/javascript" src="/assets/tinymce.js">


**4. Initialize TinyMCE**

For each textarea that you want to use with TinyMCE, add the "tinymce" class and ensure it has a unique ID:

    <%= text_area_tag :editor, "", :class => "tinymce", :rows => 40, :cols => 120 %>

Then invoke the `tinymce` helper to initialize TinyMCE:

    <%= tinymce %>

Custom options can be passed to `tinymce` to override the global options specified in `config/tinymce.yml`:

    <%= tinymce :theme => "simple", :language => "de", :plugins => ["inlinepopups", "paste"] %>


Language Packs
--------------

See the [tinymce-rails-langs](https://github.com/spohlenz/tinymce-rails-langs) gem for additional language packs for TinyMCE. The `tinymce` helper will use the current locale as the language if available, falling back to English if the core language files are missing.


Manual Initialization
---------------------

Using the `tinymce` helper and global configuration file is entirely optional. The `tinyMCE.init` function can be invoked manually if desired.

    <%= text_area_tag :editor, "", :rows => 40, :cols => 120 %>

    <script type="text/javascript">
      tinyMCE.init({
        mode: 'textareas',
        theme: 'advanced'
      });
    </script>


Custom Plugins & Skins
----------------------

To use custom plugins or skins, simply add the files to your asset load path so that they are locatable at a path beneath `tinymce/plugins/` or `tinymce/themes/advanced/skins/`.

For example, a plugin called `mycustomplugin` could have its main JS file at `app/assets/javascripts/tinymce/plugins/mycustomplugin/editor_plugin.js`.

You should also ensure that your custom paths are added to the asset precompile paths.


Using tinymce-rails as an Engine Dependency
-------------------------------------------

Ensure that you explicitly require `tinymce-rails` within your engine file. Including tinymce-rails as a dependency in your gemspec is not enough.


Updating
--------

When new versions of TinyMCE are released, simply update the `tinymce-rails` gem to the latest version. There is no need to run any extra rake tasks (apart from `rake assets:precompile`).
