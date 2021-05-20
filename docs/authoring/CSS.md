# CSS Objects

With AO 2.9.1, expanded CSS support for themes was added, allowing for theme editors to be able to fine tune their themes without the need of hacky workarounds. In this document, you'll find a list of the various Qt Classes that AO uses, as well as what objects fit under which class (and for what CSS file).

***This document is meant to be a reference, and to inform users editing CSS files what objects will be affected by adding additional parameters to the class they belong to.***
Section Names = Classes
Bullets = Objects

 - Example of targeting a ***class***:
   * QPushButton {	color: Blue;	background-color: rgba(255, 216, 0, 170);}
 - Example of targeting an ***object***:
   * *#ui_reload_theme {border: 1px solid white; background-color: rgba(0, 0, 0, 0);}
      * OR
   * QPushButton#ui_reload_theme {border: 1px solid white; background-color: rgba(0, 0, 0, 0);}

Try putting these examples into your courtroom_stylesheets.css, and watch what happens!

# courtroom_stylesheets.css

## QCheckBox
- ui_additive
- ui_casing
- ui_char_passworded
- ui_char_taken
- ui_flip
- ui_guard
- ui_immediate
- ui_pre
- ui_showname_enable

## QComboBox

- ui_emote_dropdown
- ui_effects_dropdown
- ui_iniswap_dropdown
- ui_pair_order_dropdown
- ui_pos_dropdown
- ui_sfx_dropdown
- ui_text_color

## QLabel

- ui_blip_label
- ui_music_label
- ui_sfx_label

### AOClockLabel
- ui_clock

### AOLayer (DO NOT EDIT THESE!)
#### BackgroundLayer
- ui_vp_background
- ui_vp_desk
#### CharLayer
- ui_vp_player_char
- ui_vp_sideplayer_char
#### EffectLayer
- ui_vp_effect
#### SplashLayer
- ui_vp_objection
- ui_vp_speedlines
- ui_vp_wtce
- ui_vp_testimony
#### InterfaceLayer
- ui_music_display
- ui_vp_chat_arrow
#### StickerLayer
- ui_vp_sticker

### AOEvidenceDisplay
- ui_vp_evidence_display

## QLineEdit

- ui_char_password
- ui_char_search
- ui_ic_chat_name
- ui_music_search
- ui_ooc_chat_message
- ui_ooc_chat_name

### AOLineEdit
- ui_ic_chat_message


## QListWidget

- ui_mute_list
- ui_pair_list

## QMenu
- ui_custom_obj_menu

## QPushButton
### AOButton
- AOEmoteButton
- AOCharButton
- ui_announce_casing
- ui_back_to_lobby
- ui_call_mod
- ui_change_character
- ui_char_select_left
- ui_char_select_right
- ui_cross_examination
- ui_custom_objection
- ui_defense_minus
- ui_defense_plus
- ui_emote_left
- ui_emote_right
- ui_evidence_button
- ui_guilty
- ui_hold_it
- ui_iniswap_remove
- ui_muted
- ui_not_guitly
- ui_objection
- ui_ooc_toggle
- ui_pair_button
- ui_pos_remove
- ui_prosecution_minus
- ui_prosecution_plus
- ui_realization
- ui_reload_theme
- ui_screenshake
- ui_settings
- ui_sfx_remove
- ui_spectator
- ui_switch_area_music
- ui_take_that
- ui_witness_testimony

## QSlider
- ui_music_slider
- ui_sfx_slider
- ui_blip_slider

## QSpinBox
- ui_pair_offset_spinbox
- ui_pair_vert_offset_spinbox

## QTextEdit
- ui_ic_chatlog
- ui_ooc_chatlog
- ui_vp_message

## QTreeWidget
- ui_area_list
- ui_char_list
- ui_music_list

## QWidget
- ui_emotes
- ui_char_buttons
- ui_viewport

### ScrollText
- ui_music_name

# lobby_stylesheets.css
## QLabel
- ui_player_count
### AOImage
- ui_background
- ui_loading_background
## QLineEdit
- ui_server_search
- ui_chatname
- ui_chatmessage
## QProgressBar
- ui_progress_bar
## QPushButton
### AOButton
- ui_public_servers
- ui_favorites
- ui_refresh
- ui_add_to_fav
- ui_connect
- ui_about
- ui_settings
- ui_cancel
## QTextBrowser
### AOTextArea
- ui_description
- ui_chatbox
## QTextEdit
- ui_loading_text
## QTreeWidget
- ui_server_list
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MTEwNDUyMDhdfQ==
-->