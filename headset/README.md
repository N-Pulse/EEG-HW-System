# EEG Headset Design Documentation

## Comments from Previous Years

The previous headset concepts focused on making EEG acquisition work reliably while keeping the system compatible with the decoding pipeline and the prosthesis. The main ideas were to use an adjustable structure, keep the electronics modular, and make it possible to position the electrodes consistently across users.

The designs also explored a practical division between the headset structure, the electrode system, the power board, and the processing electronics. This makes it easier to test individual parts and to replace or improve one part without redesigning the whole headset. We also studied different design approaches, including a flexible cap and a rigid cap.

## Flexible cap

![IMG_7446.HEIC](EEG%20Headset/671fdeef-2f9c-4da3-81a9-b35787eb49b4.png)

![IMG_7445](EEG%20Headset/IMG_7445.jpg)

### Pros

- Adjustable head frame
- Flexible band for electrode holders
- Simple design
- Adjustable positions for electrode holders/rows

### Cons

- Not very stable
- Hard to position well on head
- Rails of electrodes could move

## Rigid cap

![IMG_7442.jpg](EEG%20Headset/IMG_7442.jpg)

![IMG_7443.jpg](EEG%20Headset/IMG_7443.jpg)

### Pros

- Known positions for the electrodes
- Spring-loaded mounting system for electrodes
- Rear dial to adjust size
- Adapts to different head shapes through the rear-dial adjustment

### Cons

- Can create pressure points
- Battery at the forehead makes it uncomfortable
- Wires going across the whole thing

## Other Notes

- Another design we studied was the DSI-24 cap. Its umbrella structure adapts to different head shapes and is compact. We did not retain it because it did not match our electrode arrangement. See [EEG headset information](https://app.notion.com/p/EEG-headset-information-1b2e4f65ffef8061a763f2f43d6580ea?pvs=21) and [Wearable Sensing DSI-24](https://wearablesensing.com/dsi-24/).

## New design ideas

The new design combines elements from the previous years. We decided to make a rigid band that is tightened/loosened with a rear dial. We would also include a chin strap for stability and to bring the electrodes closer to the head. The electrodes will be placed in rows, made of a flexible elastic to account for head shape. They will be fixed in place to stop them from moving around. The electrode holders will also be made using the spring-loaded mounting system.

## Current CAD files

| File | Description | Format | Units | Status |
|---|---|---|---|---|
| [`frame design.step`](cad/frame%20design.step) | Current headset-frame prototype exported from Fusion 360 | STEP AP214 | mm | Work in progress |

The complete assembly and the power-board and decoding-board casing models will be added when they are ready.

## Planned purchased components

| Component | Selected product or reference | Integration status |
|---|---|---|
| Rear adjustment dial | [Selected rear-dial listing](https://www.amazon.fr/dp/B0F7X6MM9G) | Planned. Exact measurements and the attachment mechanism will be documented after the part arrives. |
| Chin strap | [ABUS chin strap Scator(-E)](https://www.galaxus.ch/en/s4/product/abus-chin-strap-scator-e-work-safety-accessories-52403525) | Planned. Exact measurements and attachment points will be documented after the part arrives. |
| EEG electrodes | [Datwyler SoftPulse products](https://datwyler.com/company/innovation/softpulse/products/) | SoftPulse Brush Long selected. Detailed holder integration is still in progress. |

## Electrode placement

- Measurement electrodes: C1, C2, C3, C4, C5, C6, Cz, FC1, FC2, FC3, FC4, CP1, CP2, CPz
- Ref: FCz
- Ground: AFz

## Electrodes used

We are using the **SoftPulse® Brush Long** dry electrodes from Datwyler.

- 12 legs, 8 mm long and 1.5 mm in diameter
- 13.5 mm total height
- 13 mm outer diameter
- 21.2 mm² contact area
- ECG standard snap

![image.png](EEG%20Headset/image.png)

[SoftPulse® Brush Long product page](https://datwyler.com/company/innovation/softpulse/products/)

## Other interesting electrodes

### Flower electrodes

Flower electrodes were considered because their flexible multi-contact shape can improve scalp contact, particularly through hair, while remaining relatively comfortable. Their geometry allows the individual contacts to adapt locally to the shape of the scalp. As dry electrodes, they also avoid the need for conductive gel, making the headset easier and faster to use.

[Research paper: Flower electrodes for comfortable dry electroencephalography](https://www.researchgate.net/publication/374418531_Flower_electrodes_for_comfortable_dry_electroencephalography)
