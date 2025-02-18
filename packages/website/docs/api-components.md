---
title: Child components
---

:::caution MapContainer required
Child components can only be used as descendants of a [MapContainer component](api-map.md#mapcontainer).
:::

## Props

Child components in React Leaflet use their props as options when creating the corresponding Leaflet instance, as described in [Leaflet's documentation](https://leafletjs.com/reference.html).

By default these props should be treated as **immutable**, only the props explicitely documented as **mutable** in this page will affect the Leaflet element when changed.

## Behaviors

Child components in React Leaflet support common behaviors described below, implementing logic related to React or Leaflet.

### Referenceable behavior

Makes the Leaflet instance for the given component accessible with the [`useRef()` hook](https://reactjs.org/docs/hooks-reference.html#useref).

```tsx Example component with Referenceable behavior
function MyComponent() {
  const circleRef = useRef()

  useEffect(() => {
    const radius = circleRef.current.getRadius()
  })

  return <Circle ref={cicleRef} center={[50.5, 30.5]} radius={200} />
}
```

### ParentComponent behavior

The component will render its mutable React children components, based on the `children?: ReactNode` prop.

```tsx Example component with ParentComponent behavior
<Marker position={[50.5, 30.5]}>
  <Popup>Hello world</Popup>
</Marker>
```

### Evented behavior

Adds support for the `eventHandlers?: LeafletEventHandlerFnMap` prop, adding and removing event listeners.

```tsx Example component with Evented behavior
<Marker
  position={[50.5, 30.5]}
  eventHandlers={{
    click: () => {
      console.log('marker clicked')
    },
  }}
/>
```

### Attribution behavior

Applies to layer components, making their [`attribution`](https://leafletjs.com/reference.html#layer-attribution) prop mutable.

```tsx Example component with Attribution behavior
<GeoJSON attribution="&copy; credits due..." data={...} />
```

### Pane behavior

Applies to layer components, adding support for the [`pane`](https://leafletjs.com/reference.html#layer-pane) prop or context from a [`Pane` component](#pane) ancestor.

```tsx Example component with pane prop
<Circle center={[50.5, 30.5]} radius={200} pane="my-existing-pane" />
```

```tsx Example component with Pane ancestor
<Pane name="custom" style={{ zIndex: 100 }}>
  <Circle center={[50.5, 30.5]} radius={200} />
</Pane>
```

### Path behavior

Applies to [vector layer components](#vector-layers), adding support for a [`pathOptions: PathOptions`](https://leafletjs.com/reference.html#path) mutable prop.

```tsx Example component with PathOptions behavior
<Circle center={[50.5, 30.5]} radius={200} pathOptions={{ color: 'blue' }} />
```

### MediaOverlay behavior

Applies to components using [Leaflet's ImageOverlay class](https://leafletjs.com/reference.html#imageoverlay), adding support for mutable [`bounds: LatLngBounds`](https://leafletjs.com/reference.html#latlngbounds), [`opacity: number`](https://leafletjs.com/reference.html#imageoverlay-opacity) and [`zIndex: number`](https://leafletjs.com/reference.html#imageoverlay-zindex) props.

```tsx Example component with MediaOverlay behavior
const bounds = new LatLngBounds([40.712216, -74.22655], [40.773941, -74.12544])

<ImageOverlay
  url="http://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg"
  bounds={bounds}
  opacity={0.5}
  zIndex={10}
/>
```

### CircleMarker behavior

Applies to components using [Leaflet's CircleMarker class](https://leafletjs.com/reference.html#circlemarker), adding support for mutable `center: LatLngExpression` and [`radius: number`](https://leafletjs.com/reference.html#circlemarker-radius) props.

```tsx Example component with CircleMarker behavior
<Circle center={[50.5, 30.5]} radius={200} />
```

### GridLayer behavior

Applies to components using [Leaflet's GridLayer class](https://leafletjs.com/reference.html#gridlayer), adding support for mutable [`opacity: number`](https://leafletjs.com/reference.html#gridlayer-opacity) and [`zIndex: number`](https://leafletjs.com/reference.html#gridlayer-zindex) props.

```tsx Example component with GridLayer behavior
<TileLayer url="..." opacity={0.5} zIndex={10} />
```

### Control behavior

Applies to [control components](#controls), making their [`position: ControlPosition`](https://leafletjs.com/reference.html#control-position) prop mutable.

```tsx Example component with Control behavior
<ZoomControl position="bottomleft" />
```

## UI layers

### Marker

[Leaflet reference](http://leafletjs.com/reference.html#marker)

**Props**

| Prop            | Type                        | Required | Mutable | Behavior                                     |
| --------------- | --------------------------- | -------- | ------- | -------------------------------------------- |
| `attribution`   | `string`                    | No       | **Yes** | [Attribution](#attribution-behavior)         |
| `children`      | `ReactNode`                 | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `draggable`     | `boolean`                   | No       | **Yes** |
| `eventHandlers` | `LeafletEventHandlerFnMap`  | No       | **Yes** | [Evented](#evented-behavior)                 |
| `icon`          | `Leaflet.Icon`              | No       | **Yes** |
| `opacity`       | `number`                    | No       | **Yes** |
| `pane`          | `string`                    | No       | No      | [Pane](#pane-behavior)                       |
| `position`      | `LatLngExpression`          | **Yes**  | **Yes** |
| `ref`           | `RefObject<Leaflet.Marker>` | No       | **Yes** | [Referenceable](#referenceable-behavior)     |
| `zIndexOffset`  | `number`                    | No       | **Yes** |

### Popup

[Leaflet reference](http://leafletjs.com/reference.html#popup)

**Props**

| Prop            | Type                       | Required | Mutable | Behavior                                     |
| --------------- | -------------------------- | -------- | ------- | -------------------------------------------- |
| `attribution`   | `string`                   | No       | **Yes** | [Attribution](#attribution-behavior)         |
| `children`      | `ReactNode`                | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `eventHandlers` | `LeafletEventHandlerFnMap` | No       | **Yes** | [Evented](#evented-behavior)                 |
| `onClose`       | `() => void`               | No       | **Yes** |
| `onOpen`        | `() => void`               | No       | **Yes** |
| `pane`          | `string`                   | No       | No      | [Pane](#pane-behavior)                       |
| `position`      | `LatLngExpression`         | No       | **Yes** |
| `ref`           | `RefObject<Leaflet.Popup>` | No       | **Yes** | [Referenceable](#referenceable-behavior)     |

### Tooltip

[Leaflet reference](http://leafletjs.com/reference.html#tooltip)

**Props**

| Prop            | Type                         | Required | Mutable | Behavior                                     |
| --------------- | ---------------------------- | -------- | ------- | -------------------------------------------- |
| `attribution`   | `string`                     | No       | **Yes** | [Attribution](#attribution-behavior)         |
| `children`      | `ReactNode`                  | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `eventHandlers` | `LeafletEventHandlerFnMap`   | No       | **Yes** | [Evented](#evented-behavior)                 |
| `onClose`       | `() => void`                 | No       | **Yes** |
| `onOpen`        | `() => void`                 | No       | **Yes** |
| `pane`          | `string`                     | No       | No      | [Pane](#pane-behavior)                       |
| `position`      | `LatLngExpression`           | No       | **Yes** |
| `ref`           | `RefObject<Leaflet.Tooltip>` | No       | **Yes** | [Referenceable](#referenceable-behavior)     |

## Raster layers

### TileLayer

[Leaflet reference](https://leafletjs.com/reference.html#tilelayer)

**Props**

| Prop            | Type                           | Required | Mutable | Behavior                                 |
| --------------- | ------------------------------ | -------- | ------- | ---------------------------------------- |
| `eventHandlers` | `LeafletEventHandlerFnMap`     | No       | **Yes** | [Evented](#evented-behavior)             |
| `opacity`       | `number`                       | No       | **Yes** | [GridLayer](#gridlayer-behavior)         |
| `pane`          | `string`                       | No       | No      | [Pane](#pane-behavior)                   |
| `ref`           | `RefObject<Leaflet.TileLayer>` | No       | **Yes** | [Referenceable](#referenceable-behavior) |
| `url`           | `string`                       | **Yes**  | No      |
| `zIndex`        | `number`                       | No       | **Yes** | [GridLayer](#gridlayer-behavior)         |

### WMSTileLayer

[Leaflet reference](https://leafletjs.com/reference.html#tilelayer-wms)

**Props**

| Prop            | Type                               | Required | Mutable | Behavior                                 |
| --------------- | ---------------------------------- | -------- | ------- | ---------------------------------------- |
| `eventHandlers` | `LeafletEventHandlerFnMap`         | No       | **Yes** | [Evented](#evented-behavior)             |
| `opacity`       | `number`                           | No       | **Yes** | [GridLayer](#gridlayer-behavior)         |
| `pane`          | `string`                           | No       | No      | [Pane](#pane-behavior)                   |
| `params`        | `WMSParams`                        | No       | **Yes** |
| `ref`           | `RefObject<Leaflet.TileLayer.WMS>` | No       | **Yes** | [Referenceable](#referenceable-behavior) |
| `url`           | `string`                           | **Yes**  | No      |
| `zIndex`        | `number`                           | No       | **Yes** | [GridLayer](#gridlayer-behavior)         |

### ImageOverlay

[Leaflet reference](https://leafletjs.com/reference.html#imageoverlay)

**Props**

| Prop            | Type                              | Required | Mutable | Behavior                                     |
| --------------- | --------------------------------- | -------- | ------- | -------------------------------------------- |
| `attribution`   | `string`                          | No       | **Yes** | [Attribution](#attribution-behavior)         |
| `bounds`        | `LatLngBounds`                    | **Yes**  | **Yes** | [MediaOverlay](#mediaoverlay-behavior)       |
| `children`      | `ReactNode`                       | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `eventHandlers` | `LeafletEventHandlerFnMap`        | No       | **Yes** | [Evented](#evented-behavior)                 |
| `opacity`       | `number`                          | No       | **Yes** | [MediaOverlay](#mediaoverlay-behavior)       |
| `ref`           | `RefObject<Leaflet.ImageOverlay>` | No       | **Yes** | [Referenceable](#referenceable-behavior)     |
| `url`           | `string`                          | **Yes**  | **Yes** |
| `zIndex`        | `number`                          | No       | **Yes** | [MediaOverlay](#mediaoverlay-behavior)       |

### VideoOverlay

[Leaflet reference](https://leafletjs.com/reference.html#videooverlay)

**Props**

| Prop            | Type                                       | Required | Mutable | Behavior                                 |
| --------------- | ------------------------------------------ | -------- | ------- | ---------------------------------------- |
| `attribution`   | `string`                                   | No       | **Yes** | [Attribution](#attribution-behavior)     |
| `bounds`        | `LatLngBounds`                             | **Yes**  | **Yes** | [MediaOverlay](#mediaoverlay-behavior)   |
| `eventHandlers` | `LeafletEventHandlerFnMap`                 | No       | **Yes** | [Evented](#evented-behavior)             |
| `play`          | `boolean`                                  | No       | **Yes** |
| `ref`           | `RefObject<Leaflet.VideoOverlay>`          | No       | **Yes** | [Referenceable](#referenceable-behavior) |
| `url`           | `string`, `string[]` or `HTMLVideoElement` | **Yes**  | **Yes** |
| `zIndex`        | `number`                                   | No       | **Yes** | [MediaOverlay](#mediaoverlay-behavior)   |

## Vector layers

### Circle

[Leaflet reference](https://leafletjs.com/reference.html#circle)

**Props**

| Prop            | Type                        | Required | Mutable | Behavior                                     |
| --------------- | --------------------------- | -------- | ------- | -------------------------------------------- |
| `attribution`   | `string`                    | No       | **Yes** | [Attribution](#attribution-behavior)         |
| `center`        | `LatLngExpression`          | **Yes**  | **Yes** |
| `children`      | `ReactNode`                 | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `eventHandlers` | `LeafletEventHandlerFnMap`  | No       | **Yes** | [Evented](#evented-behavior)                 |
| `pane`          | `string`                    | No       | No      | [Pane](#pane-behavior)                       |
| `pathOptions`   | `PathOptions`               | No       | **Yes** | [Path](#path-behavior)                       |
| `radius`        | `number`                    | **Yes**  | **Yes** |
| `ref`           | `RefObject<Leaflet.Circle>` | No       | **Yes** | [Referenceable](#referenceable-behavior)     |

### CircleMarker

[Leaflet reference](https://leafletjs.com/reference.html#circlemarker)

**Props**

| Prop            | Type                              | Required | Mutable | Behavior                                     |
| --------------- | --------------------------------- | -------- | ------- | -------------------------------------------- |
| `attribution`   | `string`                          | No       | **Yes** | [Attribution](#attribution-behavior)         |
| `center`        | `LatLngExpression`                | **Yes**  | **Yes** |
| `children`      | `ReactNode`                       | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `eventHandlers` | `LeafletEventHandlerFnMap`        | No       | **Yes** | [Evented](#evented-behavior)                 |
| `pane`          | `string`                          | No       | No      | [Pane](#pane-behavior)                       |
| `pathOptions`   | `PathOptions`                     | No       | **Yes** | [Path](#path-behavior)                       |
| `radius`        | `number`                          | **Yes**  | **Yes** |
| `ref`           | `RefObject<Leaflet.CircleMarker>` | No       | **Yes** | [Referenceable](#referenceable-behavior)     |

### Polyline

[Leaflet reference](https://leafletjs.com/reference.html#polyline)

**Props**

| Prop            | Type                                           | Required | Mutable | Behavior                                     |
| --------------- | ---------------------------------------------- | -------- | ------- | -------------------------------------------- |
| `attribution`   | `string`                                       | No       | **Yes** | [Attribution](#attribution-behavior)         |
| `children`      | `ReactNode`                                    | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `eventHandlers` | `LeafletEventHandlerFnMap`                     | No       | **Yes** | [Evented](#evented-behavior)                 |
| `pane`          | `string`                                       | No       | No      | [Pane](#pane-behavior)                       |
| `pathOptions`   | `PathOptions`                                  | No       | **Yes** | [Path](#path-behavior)                       |
| `positions`     | `LatLngExpression[]` or `LatLngExpression[][]` | **Yes**  | **Yes** |
| `ref`           | `RefObject<Leaflet.Polyline>`                  | No       | **Yes** | [Referenceable](#referenceable-behavior)     |

### Polygon

[Leaflet reference](https://leafletjs.com/reference.html#polygon)

| Prop            | Type                                                                     | Required | Mutable | Behavior                                     |
| --------------- | ------------------------------------------------------------------------ | -------- | ------- | -------------------------------------------- |
| `attribution`   | `string`                                                                 | No       | **Yes** | [Attribution](#attribution-behavior)         |
| `children`      | `ReactNode`                                                              | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `eventHandlers` | `LeafletEventHandlerFnMap`                                               | No       | **Yes** | [Evented](#evented-behavior)                 |
| `pane`          | `string`                                                                 | No       | No      | [Pane](#pane-behavior)                       |
| `pathOptions`   | `PathOptions`                                                            | No       | **Yes** | [Path](#path-behavior)                       |
| `positions`     | `LatLngExpression[]`, `LatLngExpression[][]` or `LatLngExpression[][][]` | **Yes**  | **Yes** |
| `ref`           | `RefObject<Leaflet.Polygon>`                                             | No       | **Yes** | [Referenceable](#referenceable-behavior)     |

### Rectangle

[Leaflet reference](https://leafletjs.com/reference.html#rectangle)

**Props**

| Prop            | Type                           | Required | Mutable | Behavior                                     |
| --------------- | ------------------------------ | -------- | ------- | -------------------------------------------- |
| `attribution`   | `string`                       | No       | **Yes** | [Attribution](#attribution-behavior)         |
| `bounds`        | `LatLngBoundsExpression`       | **Yes**  | **Yes** |
| `children`      | `ReactNode`                    | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `eventHandlers` | `LeafletEventHandlerFnMap`     | No       | **Yes** | [Evented](#evented-behavior)                 |
| `pane`          | `string`                       | No       | No      | [Pane](#pane-behavior)                       |
| `pathOptions`   | `PathOptions`                  | No       | **Yes** | [Path](#path-behavior)                       |
| `ref`           | `RefObject<Leaflet.Rectangle>` | No       | **Yes** | [Referenceable](#referenceable-behavior)     |

### SVGOverlay

[Leaflet reference](https://leafletjs.com/reference.html#svgoverlay)

**Props**

The `attributes` must be valid [`SVGSVGElement` properties](https://developer.mozilla.org/en-US/docs/Web/API/SVGSVGElement).

| Prop          | Type                            | Required | Mutable | Behavior                                     |
| ------------- | ------------------------------- | -------- | ------- | -------------------------------------------- |
| `attributes`  | `Record<string, string>`        | No       | No      |
| `attribution` | `string`                        | No       | **Yes** | [Attribution](#attribution-behavior)         |
| `children`    | `ReactNode`                     | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `pane`        | `string`                        | No       | No      | [Pane](#pane-behavior)                       |
| `ref`         | `RefObject<Leaflet.SVGOverlay>` | No       | **Yes** | [Referenceable](#referenceable-behavior)     |

## Other layers

### LayerGroup

[Leaflet reference](https://leafletjs.com/reference.html#layergroup)

**Props**

| Prop            | Type                            | Required | Mutable | Behavior                                     |
| --------------- | ------------------------------- | -------- | ------- | -------------------------------------------- |
| `attribution`   | `string`                        | No       | **Yes** | [Attribution](#attribution-behavior)         |
| `children`      | `ReactNode`                     | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `eventHandlers` | `LeafletEventHandlerFnMap`      | No       | **Yes** | [Evented](#evented-behavior)                 |
| `pane`          | `string`                        | No       | No      | [Pane](#pane-behavior)                       |
| `ref`           | `RefObject<Leaflet.LayerGroup>` | No       | **Yes** | [Referenceable](#referenceable-behavior)     |

### FeatureGroup

[Leaflet reference](https://leafletjs.com/reference.html#featuregroup)

**Props**

| Prop            | Type                              | Required | Mutable | Behavior                                     |
| --------------- | --------------------------------- | -------- | ------- | -------------------------------------------- |
| `attribution`   | `string`                          | No       | **Yes** | [Attribution](#attribution-behavior)         |
| `children`      | `ReactNode`                       | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `eventHandlers` | `LeafletEventHandlerFnMap`        | No       | **Yes** | [Evented](#evented-behavior)                 |
| `pane`          | `string`                          | No       | No      | [Pane](#pane-behavior)                       |
| `ref`           | `RefObject<Leaflet.FeatureGroup>` | No       | **Yes** | [Referenceable](#referenceable-behavior)     |

### GeoJSON

[Leaflet reference](https://leafletjs.com/reference.html#geojson)

**Props**

| Prop            | Type                             | Required | Mutable | Behavior                                     |
| --------------- | -------------------------------- | -------- | ------- | -------------------------------------------- |
| `attribution`   | `string`                         | No       | **Yes** | [Attribution](#attribution-behavior)         |
| `children`      | `ReactNode`                      | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `data`          | `GeoJsonObject`                  | **Yes**  | No      |
| `eventHandlers` | `LeafletEventHandlerFnMap`       | No       | **Yes** | [Evented](#evented-behavior)                 |
| `pane`          | `string`                         | No       | No      | [Pane](#pane-behavior)                       |
| `ref`           | `RefObject<Leaflet.GeoJSON>`     | No       | **Yes** | [Referenceable](#referenceable-behavior)     |
| `style`         | `PathOptions` or `StyleFunction` | No       | **Yes** |                                              |

## Controls

### ZoomControl

[Leaflet reference](https://leafletjs.com/reference.html#control-zoom)

**Props**

| Prop            | Type                              | Required | Mutable | Behavior                                 |
| --------------- | --------------------------------- | -------- | ------- | ---------------------------------------- |
| `eventHandlers` | `LeafletEventHandlerFnMap`        | No       | **Yes** | [Evented](#evented-behavior)             |
| `position`      | `ControlPosition`                 | No       | **Yes** |
| `ref`           | `RefObject<Leaflet.Control.Zoom>` | No       | **Yes** | [Referenceable](#referenceable-behavior) |

### AttributionControl

[Leaflet reference](https://leafletjs.com/reference.html#control-attribution)

**Props**

| Prop            | Type                                     | Required | Mutable | Behavior                                 |
| --------------- | ---------------------------------------- | -------- | ------- | ---------------------------------------- |
| `eventHandlers` | `LeafletEventHandlerFnMap`               | No       | **Yes** | [Evented](#evented-behavior)             |
| `position`      | `ControlPosition`                        | No       | **Yes** |
| `ref`           | `RefObject<Leaflet.Control.Attribution>` | No       | **Yes** | [Referenceable](#referenceable-behavior) |

### LayersControl

[Leaflet reference](https://leafletjs.com/reference.html#control-layers)

**Props**

| Prop            | Type                                | Required | Mutable | Behavior                                 |
| --------------- | ----------------------------------- | -------- | ------- | ---------------------------------------- |
| `collapsed`     | `boolean`                           | No       | **Yes** |
| `eventHandlers` | `LeafletEventHandlerFnMap`          | No       | **Yes** | [Evented](#evented-behavior)             |
| `position`      | `ControlPosition`                   | No       | **Yes** |
| `ref`           | `RefObject<Leaflet.Control.Layers>` | No       | **Yes** | [Referenceable](#referenceable-behavior) |

### LayersControl.BaseLayer

**Props**

| Prop       | Type        | Required | Mutable | Behavior                                     |
| ---------- | ----------- | -------- | ------- | -------------------------------------------- |
| `checked`  | `boolean`   | No       | **Yes** |
| `children` | `ReactNode` | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `name`     | `string`    | **Yes**  | No      |

### LayersControl.Overlay

**Props**

| Prop       | Type        | Required | Mutable | Behavior                                     |
| ---------- | ----------- | -------- | ------- | -------------------------------------------- |
| `checked`  | `boolean`   | No       | **Yes** |
| `children` | `ReactNode` | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `name`     | `string`    | **Yes**  | No      |

### ScaleControl

[Leaflet reference](https://leafletjs.com/reference.html#control-scale)

**Props**

| Prop            | Type                              | Required | Mutable | Behavior                                 |
| --------------- | --------------------------------- | -------- | ------- | ---------------------------------------- |
| `eventHandlers` | `LeafletEventHandlerFnMap`        | No       | **Yes** | [Evented](#evented-behavior)             |
| `position`      | `ControlPosition`                 | No       | **Yes** |
| `ref`           | `RefObject<Leaflet.Control.Scale` | No       | **Yes** | [Referenceable](#referenceable-behavior) |

## Other

### Pane

[Leaflet reference](https://leafletjs.com/reference.html#map-pane)

**Props**

:::caution
The `name` prop must be unique to the pane and different from the [default Leaflet pane names](https://leafletjs.com/reference.html#map-pane)
:::

| Prop        | Type            | Required | Mutable | Behavior                                     |
| ----------- | --------------- | -------- | ------- | -------------------------------------------- |
| `children`  | `ReactNode`     | No       | **Yes** | [ParentComponent](#parentcomponent-behavior) |
| `className` | `string`        | No       | No      |
| `name`      | `string`        | **Yes**  | No      |
| `pane`      | `string`        | No       | No      | [Pane](#pane-behavior)                       |
| `style`     | `CSSProperties` | No       | No      |
