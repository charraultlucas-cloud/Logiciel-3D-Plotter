# 3Dplotter

**Prépare le G-code d'un traceur à stylo monté sur une imprimante 3D.** On glisse un fichier SVG, on choisit la feuille, les stylos et l'imprimante, et on exporte un fichier `.gcode`.

Prévu pour le module « Traceur à stylo P1S » de la Bambu Lab P1S (publié sur MakerWorld), utilisable avec d'autres imprimantes.

[English version below](#english)

![Aperçu de 3Dplotter](apercu_3Dplotter.png)

> **Logiciel expérimental.** Il y aura probablement des erreurs. Vérifiez toujours l'aperçu et le G-code, et faites un premier essai **sans stylo ni module**, en surveillant la tête et la vitre avant. Vous pouvez l'utiliser et le modifier librement.

## Ce que fait le logiciel

- **Importe des SVG** par glisser-déposer ou par bouton. On déplace, agrandit, tourne et met en miroir le dessin sur l'aperçu.
- **Aperçu fidèle du plateau** : forme réelle de la planche de l'imprimante, zone de dessin, et repère d'échelle A5, A4 ou A3 qui montre le contour d'une feuille, même quand elle dépasse du plateau.
- **Zones interdites** (en rouge, réglables en largeur et en longueur) : la tête n'y entre jamais, ni pour tracer, ni pour se mettre en route, ni pour finir. Les dimensions des plateaux viennent des profils d'OrcaSlicer.
- **Plusieurs stylos** : une couleur du dessin peut être tracée avec un stylo différent, avec une pause de changement de stylo dans le fichier. Une couleur se décoche pour ne pas être tracée.
- **Réglages du traceur** : vitesses, hauteurs du stylo, épaisseur du trait, G-code de début et de fin modifiables.
- **Simulation du tracé** directement sur l'aperçu, avec le temps estimé et des contrôles qui signalent les problèmes (trait qui déborde d'une zone interdite, hachures trop serrées…).
- **Pause pour poser le module** après l'origine des axes (option, voir plus bas).
- **Projets** : tout est enregistré dans un fichier `.3dplotter` (dessins, couleurs, feuille, imprimante, zones). Les projets du traceur d'Argile Studio (`.argile`) s'ouvrent aussi. Le dernier travail est retrouvé tout seul au démarrage suivant.
- **Hors ligne** : tout fonctionne sur l'ordinateur, sans connexion Internet et sans compte.

## Lancer

- **Windows 10 ou 11** : double-cliquer sur `3Dplotter.exe`. Le logiciel s'ouvre dans une fenêtre d'application (il utilise Microsoft Edge ou Google Chrome, déjà présents sur la plupart des PC).
- **N'importe quel ordinateur** : ouvrir `3Dplotter.html` avec Chrome, Edge, Firefox ou Safari. C'est exactement le même logiciel.

Au premier lancement, Windows peut afficher « Windows a protégé votre ordinateur », parce que le programme n'est pas signé numériquement. Cliquer sur « Informations complémentaires », puis sur « Exécuter quand même ».

## Utiliser

1. **Dessin** : ajouter un fichier SVG, puis le placer sur l'aperçu.
2. **Feuille** : tout le plateau, ou un format de feuille.
3. **Plateau** : modèle d'imprimante ou format de traceur, zones interdites.
4. **Tracé / Machine** : stylos, vitesses, hauteurs, G-code de début et de fin.
5. **Exporter le G-code**, puis lancer le fichier d'abord **sans stylo**.

Le SVG doit contenir des formes vectorielles : les textes sont à convertir en contours, les images à vectoriser (Inkscape : Chemin > Vectoriser le bitmap).

## Poser le module après l'origine des axes

Avec la P1S, la tête peut partir vers l'avant avec le module monté et appuyer sur la vitre. L'option **« Pause pour poser le module à stylo, après l'origine des axes »** (onglet Machine, décochée par défaut) règle cela. Le fichier fait alors, dans cet ordre :

1. l'origine X, puis les déplacements jusqu'au point de départ, **sans** le module ;
2. une pause (`M400 U1` sur Bambu Lab) ;
3. on pose le module à stylo, puis on reprend sur l'écran de l'imprimante ;
4. l'imprimante descend, prend le zéro Z avec le stylo et trace.

Cette fonction n'a **pas encore été essayée sur une vraie P1S** : premier essai à vide, en reprenant à la pause sans poser le module, la main près du bouton d'arrêt.

## Limites

- Le résultat n'a pas été essayé sur toutes les imprimantes.
- Le mouvement d'origine de la machine (`G28`) est exécuté par l'imprimante elle-même : le logiciel ne le contrôle pas.
- Les quelques centimètres à l'avant du plateau d'une P1S ne sont pas utilisables avec le module monté.

## Modifier le code

Le fichier `3Dplotter.html` contient le code complet du logiciel, lisible dans un éditeur de texte. `lanceur_main.go` est le petit lanceur de l'exe (Go) et `outils/extraire_plateaux.py` refait la liste des plateaux à partir d'OrcaSlicer.

## Crédits et licences

- Dimensions des plateaux, formes, zones exclues et contour des planches : extraits des profils d'imprimantes d'[OrcaSlicer](https://github.com/SoftFever/OrcaSlicer) (licence AGPL-3.0).
- Police Bricolage Grotesque (The Bricolage Grotesque Project Authors), licence SIL Open Font License 1.1, intégrée au fichier `3Dplotter.html`.
- Le reste du code a été écrit pour ce projet.

---

<a id="english"></a>

# 3Dplotter (English)

**Prepares the G-code for a pen plotter mounted on a 3D printer.** Drop an SVG file, pick the paper, the pens and the printer, then export a `.gcode` file.

Designed for the "Traceur à stylo P1S" (pen plotter) module for the Bambu Lab P1S, published on MakerWorld, usable with other printers.

> **Experimental software.** It probably still has errors. Always check the preview and the G-code, and run a first test **without the pen or the module**, watching the toolhead and the front glass. You are free to use it and modify it.

## What it does

- **Imports SVG files** by drag and drop or with a button. Move, scale, rotate and mirror the drawing on the preview.
- **Faithful build plate preview**: the real shape of the printer's plate, the drawing area, and an A5, A4 or A3 scale reference that shows the outline of a sheet even when it is larger than the plate.
- **Keep-out zones** (in red, adjustable in width and length): the toolhead never enters them, whether drawing, starting or finishing. Plate dimensions come from OrcaSlicer's printer profiles.
- **Several pens**: each color of the drawing can be drawn with a different pen, with a pen-change pause in the file. A color can be unticked so it is not drawn.
- **Plotter settings**: speeds, pen heights, line width, editable start and end G-code.
- **Drawing simulation** right on the preview, with the estimated time and checks that flag problems (a line overlapping a keep-out zone, hatching that is too tight…).
- **Pause to install the module** after homing the axes (option, see below).
- **Projects**: everything is saved in a `.3dplotter` file (drawings, colors, paper, printer, zones). Plotter projects from Argile Studio (`.argile`) can also be opened. Your last work is restored on the next start.
- **Offline**: everything runs on your computer, with no Internet connection and no account.

## Run it

- **Windows 10 or 11**: double-click `3Dplotter.exe`. The software opens in an application window (it uses Microsoft Edge or Google Chrome, already installed on most PCs).
- **Any computer**: open `3Dplotter.html` with Chrome, Edge, Firefox or Safari. It is exactly the same software.

On first launch, Windows may show "Windows protected your PC" because the program is not digitally signed. Click "More info", then "Run anyway".

## Use it

1. **Drawing**: add an SVG file, then place it on the preview.
2. **Sheet**: the whole plate, or a paper size.
3. **Plate**: printer model or plotter size, keep-out zones.
4. **Drawing settings / Machine**: pens, speeds, heights, start and end G-code.
5. **Export the G-code**, then run the file **without a pen** first.

The SVG must contain vector shapes: convert text to outlines and trace images first (Inkscape: Path > Trace Bitmap).

## Installing the module after homing the axes

On a P1S, the toolhead can move toward the front with the module mounted and press on the glass. The option **"Pause to install the pen module, after homing the axes"** (Machine tab, off by default) solves this. The file then does, in this order:

1. home X, then move to the start point, **without** the module;
2. a pause (`M400 U1` on Bambu Lab);
3. you install the pen module, then resume on the printer's screen;
4. the printer lowers, takes the Z zero with the pen and draws.

This feature has **not been tried on a real P1S yet**: do a dry run first, resuming at the pause without installing the module, with your hand near the stop button.

## Limits

- The result has not been tested on every printer.
- The machine's homing move (`G28`) is run by the printer itself: the software does not control it.
- The first few centimeters at the front of a P1S plate are not usable with the module mounted.

## Modifying the code

The `3Dplotter.html` file contains the complete code of the software, readable in a text editor. `lanceur_main.go` is the small launcher for the exe (Go) and `outils/extraire_plateaux.py` rebuilds the plate list from OrcaSlicer.

## Credits and licenses

- Plate dimensions, shapes, keep-out zones and plate outlines: extracted from the printer profiles of [OrcaSlicer](https://github.com/SoftFever/OrcaSlicer) (AGPL-3.0 license).
- Bricolage Grotesque font (The Bricolage Grotesque Project Authors), SIL Open Font License 1.1, embedded in `3Dplotter.html`.
- The rest of the code was written for this project.
