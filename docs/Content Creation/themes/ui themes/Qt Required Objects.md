# CSS Objects

With AO 2.10.1, UI files and Ressource Files was added, allowing for theme editors to customise the entire UI even further than stylesheets could without the need of hacky workarounds. This however means that objects inside the UI file can be named, but some are required for the client to operate correctly.

The objects below need to be present anywhere in the UI file, though do not require a specific position.

## lobby.ui

Lobby, also often refered to as Serverlist, is the first window that open when you start the client.

| Type        | Name                          |
|-------------|-------------------------------|
| Label       | game_version_lbl;             |
| Label       | server_player_count_lbl;      |
| TextBrowser | server_description_text;      |
| TextBrowser | motd_text;                    |
| PushButton  | settings_button;              |
| PushButton  | about_button;                 |
| TabWidget   | connections_tabview;          |
| TreeWidget  | serverlist_tree;              |
| TreeWidget  | favorites_tree;               |
| TreeWidget  | demo_tree;                    |
| LineEdit    | demo_search;                  |
| LineEdit    | serverlist_search;            |
| PushButton  | add_to_favorite_button;       |
| PushButton  | add_server_button;            |
| PushButton  | remove_from_favorites_button; |
| PushButton  | edit_favorite_button;         |
| PushButton  | direct_connect_button;        |
| PushButton  | refresh_button;               |
| PushButton  | connect_button;               |

## options_dialog.ui

| Type            | Name                          |
|-----------------|-------------------------------|
| DialogButtonBox | settings_buttons              |
| Widget          | settings_widget               |
| ComboBox        | theme_combobox                |
| ComboBox        | subtheme_combobox             |
| PushButton      | theme_reload_button           |
| PushButton      | theme_folder_button           |
| CheckBox        | evidence_double_click_cb      |
| CheckBox        | animated_theme_cb             |
| SpinBox         | stay_time_spinbox             |
| CheckBox        | instant_objection_cb          |
| SpinBox         | text_crawl_spinbox            |
| SpinBox         | chat_ratelimit_spinbox        |
| Frame           | log_names_divider             |
| LineEdit        | username_textbox              |
| CheckBox        | showname_cb                   |
| LineEdit        | default_showname_textbox      |
| Frame           | net_divider                   |
| LineEdit        | ms_textbox                    |
| CheckBox        | discord_cb                    |
| Label           | language_label                |
| ComboBox        | language_combobox             |
| Label           | scaling_label                 |
| ComboBox        | scaling_combobox              |
| CheckBox        | shake_cb                      |
| CheckBox        | effects_cb                    |
| CheckBox        | framenetwork_cb               |
| CheckBox        | colorlog_cb                   |
| CheckBox        | stickysounds_cb               |
| CheckBox        | stickyeffects_cb              |
| CheckBox        | stickypres_cb                 |
| CheckBox        | customchat_cb                 |
| CheckBox        | sticker_cb                    |
| CheckBox        | continuous_cb                 |
| CheckBox        | category_stop_cb              |
| CheckBox        | sfx_on_idle_cb                |
| PlainTextEdit   | callwords_textbox             |
| CheckBox        | callwords_char_textbox        |
| Widget          | audio_tab                     |
| Widget          | audio_widget                  |
| ComboBox        | audio_device_combobox         |
| SpinBox         | music_volume_spinbox          |
| SpinBox         | sfx_volume_spinbox            |
| SpinBox         | blips_volume_spinbox          |
| SpinBox         | suppress_audio_spinbox        |
| Frame           | volume_blip_divider           |
| SpinBox         | bliprate_spinbox              |
| CheckBox        | blank_blips_cb                |
| CheckBox        | loopsfx_cb                    |
| CheckBox        | objectmusic_cb                |
| CheckBox        | disablestreams_cb             |
| ListWidget      | mount_list                    |
| PushButton      | mount_add                     |
| PushButton      | mount_remove                  |
| PushButton      | mount_up                      |
| PushButton      | mount_down                    |
| PushButton      | mount_clear_cache             |
| CheckBox        | downwards_cb                  |
| SpinBox         | length_spinbox                |
| CheckBox        | log_newline_cb                |
| SpinBox         | log_margin_spinbox            |
| Label           | log_timestamp_format_lbl      |
| CheckBox        | log_timestamp_cb              |
| ComboBox        | log_timestamp_format_combobox |
| CheckBox        | desync_logs_cb                |
| CheckBox        | log_ic_actions_cb             |
| CheckBox        | log_text_cb                   |
| CheckBox        | log_demo_cb                   |
| Widget          | privacy_tab                   |
| CheckBox        | privacy_optout_cb             |
| Frame           | privacy_separator             |
| TextBrowser     | privacy_policy                |