# Home Assistant LCARS
Star Trek LCARS theme for Home Assistant



## Example dashboard
![image](https://user-images.githubusercontent.com/38670315/210180806-88684ec3-283f-4fe2-8adc-024b1ad69133.png "Automations for hot Earl Grey Tea not included.")
Color codes and font choice from https://www.thelcars.com
    --thanks Jim Robertus!
    
## Included themes
![image](https://user-images.githubusercontent.com/38670315/206508344-043c00ad-2407-4159-a1ee-1800dbea3e2a.png "Classic, Lower Decks, Nemesis, Romulus, Kronos.")
    
## Preamble
I am most definitely not a real web developer, and fumbled my way into the initial release with the help of Stack Exchange and various blogs on CSS techniques. My main goal was and still is to keep this theme 100% CSS with no extra assets required besides the font. I'm positive there are better ways to implement anything and everything I've done thus far, so PRs are welcome. I will continue to improve things as I learn and add more comments to my CSS so that you can know what things do and maybe tell me how it can be better if you know. I have tested this theme with most of the out-of-the-box cards that ship with Home Assistant, and some available in HACS like the Mail and Packages card. However, I'm sure there are some that could still be terribly broken. Simply create an issue and I will address it. 

## Installation instructions
### Prerequisites
#### I. Enable themes and install card-mod

1. Install `card-mod` per the instructions on its [GitHub page](https://github.com/thomasloven/lovelace-card-mod "card-mod").

2. Make sure in your **configuration.yaml** file you have the following:
```yaml
frontend:
  javascript_version: latest
  themes: !include_dir_merge_named themes
  extra_module_url:
    - /local/community/card-mod.js #or wherever you ended up putting card-mod.js
```
3. Under the Home Assistant **Config** folder, create a new folder named **themes**. 
4. **Restart** Home assistant to apply the changes.

#### II. Add the font
This theme requires you to add the `Antonio` font as a resource to your lovelace configuration:
```yaml
resources:
  - url: https://fonts.googleapis.com/css2?family=Antonio:wght@400;700&display=swap
  type: css
```
##### -OR-
Navigate to `Settings` → `Dashboards` → `3-dot menu` → `Resources` and add a new Resource with the above URL and selected as a stylesheet.

#### III. Set up the clock
In order for the clock to work, you need to set up the Time & Date integration by adding the following to your configuration.yaml:
```yaml
sensor:
  - platform: time_date
    display_options:
      - 'time'
      - 'date'
      - 'date_time'
      - 'date_time_utc'
      - 'date_time_iso'
      - 'time_date'
      - 'time_utc'
      - 'beat'
```

More info:
https://www.home-assistant.io/integrations/time_date/

### Install the theme
Install via HACS by adding this repository URL or download the [latest release](https://github.com/th3jesta/ha-lcars/releases/latest) and extract and drop the lcars folder into your themes folder.

### Enable theme
#### Option 1: Via profile
1. Open your Home Assistant **Profile**
2. Under, **Themes**, select one of the new LCARS themes
3. Call the `frontend.reload_themes` service.

#### Option 2: Setting the default `backend-selected` theme
In order to have this theme set automatically as the backend selected default, add the following automation to your Home Assistant:
```yaml
- alias: Set Default Theme
  description: ''
  trigger:
  - event: start
    platform: homeassistant
  condition: []
  action:
  - data:
      name: LCARS Default # or whichever other theme is available, like LCARS Lower Decks
    service: frontend.set_theme
```
## Usage instructions
### Classes
The theme includes some classes that can be added to cards like this to give them special styling:
```yaml
card_mod:
  class: header
```
_The class names are only indications of what types of cards they were intended for, but the classes can be applied to any card you like. I cannot guarantee how well they will work outside of their intended uses, however._

The classes are as follows:
1. `header` and `header-right` - top blue bar (in Default theme) meant for Markdown cards with one `H1` line that will start a section
<img width="319" alt="image" src="https://user-images.githubusercontent.com/38670315/210178336-ee6ca541-31c8-48bf-b1d8-e2069d435111.png">

2. `middle` and `middle-right` -  side red bar (in Default theme) meant for non-button sections below `header` and above `footer`
<img width="319" alt="image" src="https://user-images.githubusercontent.com/38670315/210178353-8fab9352-a3f5-4105-aea0-b05ede7f3cdd.png">

3. `middle-blank` - special case for Mushroom Cards (@csanner please confirm)

4. `footer` and `footer-right` - bottom gray bar (in Default theme) meant for the last card in a section
<img width="317" alt="image" src="https://user-images.githubusercontent.com/38670315/210178378-7b45ec2d-30fc-45c9-a6ab-68f2517a83fc.png">

5. `button-small` - squared off buttons intended to go in middle sections
<img width="107" alt="image" src="https://user-images.githubusercontent.com/38670315/210178400-5d39e821-328f-4bd5-907d-1863dc2f7ff6.png">

6. `button-large` - rounded button meant to be standalone outside of `header`-`middle`-`footer` sections
<img width="318" alt="image" src="https://user-images.githubusercontent.com/38670315/210178438-59da5bce-4f86-4de6-94e3-830aa845293e.png">

7. `button-lozenge` - pill-shaped button; only works on standard button cards; also works on button cards in a horizontal-stack card that's two columns wide
<img width="158" alt="image" src="https://user-images.githubusercontent.com/38670315/210178574-68183ee3-a3a8-4642-85a6-7d0f1dfed537.png">

8. `button-bullet` - similar to the lozenge, but with a squared-off left side
<img width="157" alt="image" src="https://user-images.githubusercontent.com/38670315/210178586-7f339168-4c6d-4ed7-aa80-cdacd3b57194.png">

### Make your own color themes
Custom themes can be created down at the bottom of `lcars.yaml`. Or, search for "===THEMES", which will take you right there. To create your own theme, copy the LCARS Default section to the bottom of the file and change the `lcars-ui-*` and `lcars-card-*` variables to your liking, using the color references at the top of the file, [The LCARS website](https://www.thelcars.com/colors.php), or define your own.

### Noise and gradients
If you're not feeling the subtle noise and gradients that this theme added, let me know by raising an issue. I am still working on an easy method to enable and disable them. You can remove them yourself by searching for "base64" and "-gradient" (you should find 3 entries each) and commenting/deleting the entire CSS blocks that contain them. [Feature request here](https://github.com/th3jesta/ha-lcars/issues/5).

## Tips and tricks
_If you have anything to add here, create a PR with your tip and I will review it to add to this list._
* Make use of Vertical Stack cards. Whether in this theme or any other theme, they are invaluable for keeping dashboards organized. In LCARS, a Vertical Stack card should contain a Markdown card first with the title of the group and the `header` class applied, then any number of `middle` class cards and `button` class single buttons or in horizontal stacks or grids, and then finally a `footer` class applied to the last card in the vertical stack. You can see this formation in all of the screenshots at the top of this page.

## Known issues
* Font and sidebar and header CSS styles only load when a dashboard has been loaded first. If you navigate directly to a non-dashboard page without loading a dashboard first, things will look pretty awful, though still functional. Simply load a dashboard and hit the back button. This is a quirk of the [card-mod](https://github.com/thomasloven/lovelace-card-mod) addon on which this theme relies, so it's outside my ability to fix.
* card-mod classes do not work with Vertical Stack and Horizontal Stacks cards (though they do work with the cards they contain). This is a quirk of the [card-mod](https://github.com/thomasloven/lovelace-card-mod) addon on which this theme relies, so it's outside my ability to fix. There is, however, a hacky workaround I have identified though have opted to not include at this time. Please submit a feature request if you would like to see this included.
* Collapsing and expanding the sidebar or zooming the interface will jack with the noise and gradient overlays. A simple refresh will set everything right again. I hope to find a way to make the pseudo elements that contain the textures to dynamically adjust with the DOM. PRs are welcome. [Bug here](https://github.com/th3jesta/ha-lcars/issues/4).
* Menu pages like Development Tools and Profile are functional, but not great. Unfortunately, there's not much I can do to address this as card-mod does not provide a method to change these pages. Issues raised for anything comepletely broken and unsuable that I may have missed are welcome, as are PRs to make to make things better.
* Sometimes when a dashboard loads, not all CSS styles will load and all or most cards will end up looking like the `button-large` cards. This is more prevalent on large dashboards. Try reloading the page, and if that doesn't work, load a smaller dashboard and then go back to the offending dashboard.

## Acknowledgements
- Thanks to @JHuckins for color theming support and testing!
- Thanks to @csanner for the new classes and additional fixes and tweaks!
