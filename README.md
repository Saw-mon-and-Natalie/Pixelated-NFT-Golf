# A New Method to Render NFT Pixel Arts
## Proposal
Most on-chain pixel art NFTs use `SVG` format to render the image using the `rect` tag. Here we present an alternative method using `CSS`'s `box-shadow` property. The following alternative would use fewer characters. Basically a structure like
```html
<rect x="X" y="Y" width="1 height="1"" class="ci" />
```
will be turned into:
```css
Xpx Ypx var(--i),
```

### Benefits

- A small amount of gas will be saved when deploying a renderer contract. 
- A big amount of memory expansion will be saved for a node calculating the data URL. Although most `render`/`tokenURI` functions are `view` functions. This would just be computationally beneficial for the nodes rendering it (would require less resources).

## Implementation

1. Draw a `1px x 1px` using an `html` tag like `div`:
```html
<div />
<style>
   div {
      display: inline-block;
      width: 1px;
      height: 1px;
}
</style>
```
2. Add your color palette using `CSS` variables:
```html
<div />
<style>
   div {
      display: inline-block;
      width: 1px;
      height: 1px;
      --0: #XXXXXX;
      ...
      --n: #XXXXXX;
}
</style>
```
3. Draw each pixel using its coordinates and its color from the palette using the `box-shadow` property:
```css
      box-shadow:
            ...
            xi yi var(--m),            /* xi and yi are the coordinates of the pixel and --i is the ith color */
            ...
```

4. Scale the drawing:
```css
      transform: scale(X);             /* X is the scale factor */
      transform-origin: top left;
```

The final snippet would look like this before removing any extra whitespaces:
```html
<div />

<style>
div {
  display: inline-block;
  width: 1px;
  height: 1px;
  --0: #XXXXXX;
  ...
  --n: #XXXXXX;
  background: var(--j);
  transform: scale(X);             /* X is the scale factor */
  transform-origin: top left;
  box-shadow: 
    ...
    xi yi var(--m),                /* xi and yi are the coordinates of the pixel and --m is the ith color */
    ...
}
</style>
```


## Example

Data URI for [`Mirakai Scroll`](https://github.com/0xBeans/Mirakai):
```
data:text/html;base64,PGRpdiAvPjxzdHlsZT5kaXYgey0tMDp0cmFuc3BhcmVudDstLTE6IzhiMzYxNTstLTI6I2Q0OTQ0MzstLTM6I2M1NzAzMjstLTQ6Izc2MjkwYzt3aWR0aDoxcHg7aGVpZ2h0OjFweDtiYWNrZ3JvdW5kOnZhcigtLTApO2Rpc3BsYXk6IGlubGluZS1ibG9jazt0cmFuc2Zvcm06c2NhbGUoMTApO3RyYW5zZm9ybS1vcmlnaW46dG9wIGxlZnQ7Ym94LXNoYWRvdzowcHggMHB4IHZhcigtLTApLDFweCAwcHggdmFyKC0tMCksMnB4IDBweCB2YXIoLS0wKSwzcHggMHB4IHZhcigtLTEpLDRweCAwcHggdmFyKC0tMSksNXB4IDBweCB2YXIoLS0xKSw2cHggMHB4IHZhcigtLTEpLDdweCAwcHggdmFyKC0tMSksOHB4IDBweCB2YXIoLS0xKSw5cHggMHB4IHZhcigtLTEpLDEwcHggMHB4IHZhcigtLTEpLDExcHggMHB4IHZhcigtLTEpLDEycHggMHB4IHZhcigtLTEpLDEzcHggMHB4IHZhcigtLTEpLDE0cHggMHB4IHZhcigtLTEpLDE1cHggMHB4IHZhcigtLTEpLDE2cHggMHB4IHZhcigtLTEpLDE3cHggMHB4IHZhcigtLTEpLDE4cHggMHB4IHZhcigtLTEpLDE5cHggMHB4IHZhcigtLTEpLDIwcHggMHB4IHZhcigtLTApLDIxcHggMHB4IHZhcigtLTApLDIycHggMHB4IHZhcigtLTApLDIzcHggMHB4IHZhcigtLTApLDBweCAxcHggdmFyKC0tMCksMXB4IDFweCB2YXIoLS0wKSwycHggMXB4IHZhcigtLTEpLDNweCAxcHggdmFyKC0tMiksNHB4IDFweCB2YXIoLS0yKSw1cHggMXB4IHZhcigtLTIpLDZweCAxcHggdmFyKC0tMiksN3B4IDFweCB2YXIoLS0yKSw4cHggMXB4IHZhcigtLTIpLDlweCAxcHggdmFyKC0tMiksMTBweCAxcHggdmFyKC0tMiksMTFweCAxcHggdmFyKC0tMiksMTJweCAxcHggdmFyKC0tMiksMTNweCAxcHggdmFyKC0tMiksMTRweCAxcHggdmFyKC0tMiksMTVweCAxcHggdmFyKC0tMiksMTZweCAxcHggdmFyKC0tMiksMTdweCAxcHggdmFyKC0tMiksMThweCAxcHggdmFyKC0tMiksMTlweCAxcHggdmFyKC0tMiksMjBweCAxcHggdmFyKC0tMSksMjFweCAxcHggdmFyKC0tMCksMjJweCAxcHggdmFyKC0tMCksMjNweCAxcHggdmFyKC0tMCksMHB4IDJweCB2YXIoLS0wKSwxcHggMnB4IHZhcigtLTApLDJweCAycHggdmFyKC0tMSksM3B4IDJweCB2YXIoLS0yKSw0cHggMnB4IHZhcigtLTMpLDVweCAycHggdmFyKC0tMyksNnB4IDJweCB2YXIoLS0zKSw3cHggMnB4IHZhcigtLTMpLDhweCAycHggdmFyKC0tMyksOXB4IDJweCB2YXIoLS0zKSwxMHB4IDJweCB2YXIoLS0zKSwxMXB4IDJweCB2YXIoLS0zKSwxMnB4IDJweCB2YXIoLS0zKSwxM3B4IDJweCB2YXIoLS0zKSwxNHB4IDJweCB2YXIoLS0zKSwxNXB4IDJweCB2YXIoLS0zKSwxNnB4IDJweCB2YXIoLS0zKSwxN3B4IDJweCB2YXIoLS0zKSwxOHB4IDJweCB2YXIoLS0zKSwxOXB4IDJweCB2YXIoLS0yKSwyMHB4IDJweCB2YXIoLS0xKSwyMXB4IDJweCB2YXIoLS0wKSwyMnB4IDJweCB2YXIoLS0wKSwyM3B4IDJweCB2YXIoLS0wKSwwcHggM3B4IHZhcigtLTApLDFweCAzcHggdmFyKC0tMCksMnB4IDNweCB2YXIoLS0wKSwzcHggM3B4IHZhcigtLTEpLDRweCAzcHggdmFyKC0tMSksNXB4IDNweCB2YXIoLS0xKSw2cHggM3B4IHZhcigtLTEpLDdweCAzcHggdmFyKC0tMSksOHB4IDNweCB2YXIoLS0xKSw5cHggM3B4IHZhcigtLTEpLDEwcHggM3B4IHZhcigtLTEpLDExcHggM3B4IHZhcigtLTEpLDEycHggM3B4IHZhcigtLTEpLDEzcHggM3B4IHZhcigtLTEpLDE0cHggM3B4IHZhcigtLTEpLDE1cHggM3B4IHZhcigtLTEpLDE2cHggM3B4IHZhcigtLTEpLDE3cHggM3B4IHZhcigtLTEpLDE4cHggM3B4IHZhcigtLTEpLDE5cHggM3B4IHZhcigtLTEpLDIwcHggM3B4IHZhcigtLTApLDIxcHggM3B4IHZhcigtLTApLDIycHggM3B4IHZhcigtLTApLDIzcHggM3B4IHZhcigtLTApLDBweCA0cHggdmFyKC0tMCksMXB4IDRweCB2YXIoLS0wKSwycHggNHB4IHZhcigtLTApLDNweCA0cHggdmFyKC0tNCksNHB4IDRweCB2YXIoLS0zKSw1cHggNHB4IHZhcigtLTMpLDZweCA0cHggdmFyKC0tMyksN3B4IDRweCB2YXIoLS0zKSw4cHggNHB4IHZhcigtLTMpLDlweCA0cHggdmFyKC0tMyksMTBweCA0cHggdmFyKC0tMyksMTFweCA0cHggdmFyKC0tMyksMTJweCA0cHggdmFyKC0tMyksMTNweCA0cHggdmFyKC0tMyksMTRweCA0cHggdmFyKC0tMyksMTVweCA0cHggdmFyKC0tMyksMTZweCA0cHggdmFyKC0tMyksMTdweCA0cHggdmFyKC0tMyksMThweCA0cHggdmFyKC0tMyksMTlweCA0cHggdmFyKC0tNCksMjBweCA0cHggdmFyKC0tMCksMjFweCA0cHggdmFyKC0tMCksMjJweCA0cHggdmFyKC0tMCksMjNweCA0cHggdmFyKC0tMCksMHB4IDVweCB2YXIoLS0wKSwxcHggNXB4IHZhcigtLTApLDJweCA1cHggdmFyKC0tMCksM3B4IDVweCB2YXIoLS00KSw0cHggNXB4IHZhcigtLTIpLDVweCA1cHggdmFyKC0tMiksNnB4IDVweCB2YXIoLS0yKSw3cHggNXB4IHZhcigtLTIpLDhweCA1cHggdmFyKC0tMiksOXB4IDVweCB2YXIoLS0yKSwxMHB4IDVweCB2YXIoLS0yKSwxMXB4IDVweCB2YXIoLS0yKSwxMnB4IDVweCB2YXIoLS0yKSwxM3B4IDVweCB2YXIoLS0yKSwxNHB4IDVweCB2YXIoLS0yKSwxNXB4IDVweCB2YXIoLS0yKSwxNnB4IDVweCB2YXIoLS0yKSwxN3B4IDVweCB2YXIoLS0yKSwxOHB4IDVweCB2YXIoLS0zKSwxOXB4IDVweCB2YXIoLS00KSwyMHB4IDVweCB2YXIoLS0wKSwyMXB4IDVweCB2YXIoLS0wKSwyMnB4IDVweCB2YXIoLS0wKSwyM3B4IDVweCB2YXIoLS0wKSwwcHggNnB4IHZhcigtLTApLDFweCA2cHggdmFyKC0tMCksMnB4IDZweCB2YXIoLS0wKSwzcHggNnB4IHZhcigtLTQpLDRweCA2cHggdmFyKC0tMiksNXB4IDZweCB2YXIoLS0yKSw2cHggNnB4IHZhcigtLTIpLDdweCA2cHggdmFyKC0tMiksOHB4IDZweCB2YXIoLS0yKSw5cHggNnB4IHZhcigtLTIpLDEwcHggNnB4IHZhcigtLTIpLDExcHggNnB4IHZhcigtLTIpLDEycHggNnB4IHZhcigtLTIpLDEzcHggNnB4IHZhcigtLTIpLDE0cHggNnB4IHZhcigtLTIpLDE1cHggNnB4IHZhcigtLTIpLDE2cHggNnB4IHZhcigtLTIpLDE3cHggNnB4IHZhcigtLTIpLDE4cHggNnB4IHZhcigtLTMpLDE5cHggNnB4IHZhcigtLTQpLDIwcHggNnB4IHZhcigtLTApLDIxcHggNnB4IHZhcigtLTApLDIycHggNnB4IHZhcigtLTApLDIzcHggNnB4IHZhcigtLTApLDBweCA3cHggdmFyKC0tMCksMXB4IDdweCB2YXIoLS0wKSwycHggN3B4IHZhcigtLTApLDNweCA3cHggdmFyKC0tMSksNHB4IDdweCB2YXIoLS0yKSw1cHggN3B4IHZhcigtLTIpLDZweCA3cHggdmFyKC0tMiksN3B4IDdweCB2YXIoLS0yKSw4cHggN3B4IHZhcigtLTIpLDlweCA3cHggdmFyKC0tMiksMTBweCA3cHggdmFyKC0tMiksMTFweCA3cHggdmFyKC0tMiksMTJweCA3cHggdmFyKC0tMiksMTNweCA3cHggdmFyKC0tMiksMTRweCA3cHggdmFyKC0tMiksMTVweCA3cHggdmFyKC0tMiksMTZweCA3cHggdmFyKC0tMiksMTdweCA3cHggdmFyKC0tMiksMThweCA3cHggdmFyKC0tMyksMTlweCA3cHggdmFyKC0tNCksMjBweCA3cHggdmFyKC0tMCksMjFweCA3cHggdmFyKC0tMCksMjJweCA3cHggdmFyKC0tMCksMjNweCA3cHggdmFyKC0tMCksMHB4IDhweCB2YXIoLS0wKSwxcHggOHB4IHZhcigtLTApLDJweCA4cHggdmFyKC0tMCksM3B4IDhweCB2YXIoLS00KSw0cHggOHB4IHZhcigtLTIpLDVweCA4cHggdmFyKC0tMiksNnB4IDhweCB2YXIoLS0yKSw3cHggOHB4IHZhcigtLTIpLDhweCA4cHggdmFyKC0tMiksOXB4IDhweCB2YXIoLS0yKSwxMHB4IDhweCB2YXIoLS0yKSwxMXB4IDhweCB2YXIoLS0yKSwxMnB4IDhweCB2YXIoLS0yKSwxM3B4IDhweCB2YXIoLS0yKSwxNHB4IDhweCB2YXIoLS0yKSwxNXB4IDhweCB2YXIoLS0yKSwxNnB4IDhweCB2YXIoLS0yKSwxN3B4IDhweCB2YXIoLS0yKSwxOHB4IDhweCB2YXIoLS0zKSwxOXB4IDhweCB2YXIoLS00KSwyMHB4IDhweCB2YXIoLS0wKSwyMXB4IDhweCB2YXIoLS0wKSwyMnB4IDhweCB2YXIoLS0wKSwyM3B4IDhweCB2YXIoLS0wKSwwcHggOXB4IHZhcigtLTApLDFweCA5cHggdmFyKC0tMCksMnB4IDlweCB2YXIoLS0wKSwzcHggOXB4IHZhcigtLTEpLDRweCA5cHggdmFyKC0tMiksNXB4IDlweCB2YXIoLS0yKSw2cHggOXB4IHZhcigtLTIpLDdweCA5cHggdmFyKC0tMiksOHB4IDlweCB2YXIoLS0yKSw5cHggOXB4IHZhcigtLTIpLDEwcHggOXB4IHZhcigtLTIpLDExcHggOXB4IHZhcigtLTIpLDEycHggOXB4IHZhcigtLTIpLDEzcHggOXB4IHZhcigtLTIpLDE0cHggOXB4IHZhcigtLTIpLDE1cHggOXB4IHZhcigtLTIpLDE2cHggOXB4IHZhcigtLTIpLDE3cHggOXB4IHZhcigtLTIpLDE4cHggOXB4IHZhcigtLTMpLDE5cHggOXB4IHZhcigtLTQpLDIwcHggOXB4IHZhcigtLTApLDIxcHggOXB4IHZhcigtLTApLDIycHggOXB4IHZhcigtLTApLDIzcHggOXB4IHZhcigtLTApLDBweCAxMHB4IHZhcigtLTApLDFweCAxMHB4IHZhcigtLTApLDJweCAxMHB4IHZhcigtLTApLDNweCAxMHB4IHZhcigtLTEpLDRweCAxMHB4IHZhcigtLTIpLDVweCAxMHB4IHZhcigtLTIpLDZweCAxMHB4IHZhcigtLTIpLDdweCAxMHB4IHZhcigtLTIpLDhweCAxMHB4IHZhcigtLTIpLDlweCAxMHB4IHZhcigtLTIpLDEwcHggMTBweCB2YXIoLS0yKSwxMXB4IDEwcHggdmFyKC0tMiksMTJweCAxMHB4IHZhcigtLTIpLDEzcHggMTBweCB2YXIoLS0yKSwxNHB4IDEwcHggdmFyKC0tMiksMTVweCAxMHB4IHZhcigtLTIpLDE2cHggMTBweCB2YXIoLS0yKSwxN3B4IDEwcHggdmFyKC0tMiksMThweCAxMHB4IHZhcigtLTMpLDE5cHggMTBweCB2YXIoLS00KSwyMHB4IDEwcHggdmFyKC0tMCksMjFweCAxMHB4IHZhcigtLTApLDIycHggMTBweCB2YXIoLS0wKSwyM3B4IDEwcHggdmFyKC0tMCksMHB4IDExcHggdmFyKC0tMCksMXB4IDExcHggdmFyKC0tMCksMnB4IDExcHggdmFyKC0tMCksM3B4IDExcHggdmFyKC0tMCksNHB4IDExcHggdmFyKC0tMSksNXB4IDExcHggdmFyKC0tMiksNnB4IDExcHggdmFyKC0tMiksN3B4IDExcHggdmFyKC0tMiksOHB4IDExcHggdmFyKC0tMiksOXB4IDExcHggdmFyKC0tMiksMTBweCAxMXB4IHZhcigtLTIpLDExcHggMTFweCB2YXIoLS0yKSwxMnB4IDExcHggdmFyKC0tMiksMTNweCAxMXB4IHZhcigtLTIpLDE0cHggMTFweCB2YXIoLS0yKSwxNXB4IDExcHggdmFyKC0tMiksMTZweCAxMXB4IHZhcigtLTIpLDE3cHggMTFweCB2YXIoLS0yKSwxOHB4IDExcHggdmFyKC0tMyksMTlweCAxMXB4IHZhcigtLTQpLDIwcHggMTFweCB2YXIoLS0wKSwyMXB4IDExcHggdmFyKC0tMCksMjJweCAxMXB4IHZhcigtLTApLDIzcHggMTFweCB2YXIoLS0wKSwwcHggMTJweCB2YXIoLS0wKSwxcHggMTJweCB2YXIoLS0wKSwycHggMTJweCB2YXIoLS0wKSwzcHggMTJweCB2YXIoLS0xKSw0cHggMTJweCB2YXIoLS0yKSw1cHggMTJweCB2YXIoLS0yKSw2cHggMTJweCB2YXIoLS0yKSw3cHggMTJweCB2YXIoLS0yKSw4cHggMTJweCB2YXIoLS0yKSw5cHggMTJweCB2YXIoLS0yKSwxMHB4IDEycHggdmFyKC0tMiksMTFweCAxMnB4IHZhcigtLTIpLDEycHggMTJweCB2YXIoLS0yKSwxM3B4IDEycHggdmFyKC0tMiksMTRweCAxMnB4IHZhcigtLTIpLDE1cHggMTJweCB2YXIoLS0yKSwxNnB4IDEycHggdmFyKC0tMiksMTdweCAxMnB4IHZhcigtLTIpLDE4cHggMTJweCB2YXIoLS0zKSwxOXB4IDEycHggdmFyKC0tNCksMjBweCAxMnB4IHZhcigtLTApLDIxcHggMTJweCB2YXIoLS0wKSwyMnB4IDEycHggdmFyKC0tMCksMjNweCAxMnB4IHZhcigtLTApLDBweCAxM3B4IHZhcigtLTApLDFweCAxM3B4IHZhcigtLTApLDJweCAxM3B4IHZhcigtLTApLDNweCAxM3B4IHZhcigtLTEpLDRweCAxM3B4IHZhcigtLTIpLDVweCAxM3B4IHZhcigtLTIpLDZweCAxM3B4IHZhcigtLTIpLDdweCAxM3B4IHZhcigtLTIpLDhweCAxM3B4IHZhcigtLTIpLDlweCAxM3B4IHZhcigtLTIpLDEwcHggMTNweCB2YXIoLS0yKSwxMXB4IDEzcHggdmFyKC0tMiksMTJweCAxM3B4IHZhcigtLTIpLDEzcHggMTNweCB2YXIoLS0yKSwxNHB4IDEzcHggdmFyKC0tMiksMTVweCAxM3B4IHZhcigtLTIpLDE2cHggMTNweCB2YXIoLS0yKSwxN3B4IDEzcHggdmFyKC0tMiksMThweCAxM3B4IHZhcigtLTMpLDE5cHggMTNweCB2YXIoLS0xKSwyMHB4IDEzcHggdmFyKC0tMCksMjFweCAxM3B4IHZhcigtLTApLDIycHggMTNweCB2YXIoLS0wKSwyM3B4IDEzcHggdmFyKC0tMCksMHB4IDE0cHggdmFyKC0tMCksMXB4IDE0cHggdmFyKC0tMCksMnB4IDE0cHggdmFyKC0tMCksM3B4IDE0cHggdmFyKC0tNCksNHB4IDE0cHggdmFyKC0tMiksNXB4IDE0cHggdmFyKC0tMiksNnB4IDE0cHggdmFyKC0tMiksN3B4IDE0cHggdmFyKC0tMiksOHB4IDE0cHggdmFyKC0tMiksOXB4IDE0cHggdmFyKC0tMiksMTBweCAxNHB4IHZhcigtLTIpLDExcHggMTRweCB2YXIoLS0yKSwxMnB4IDE0cHggdmFyKC0tMiksMTNweCAxNHB4IHZhcigtLTIpLDE0cHggMTRweCB2YXIoLS0yKSwxNXB4IDE0cHggdmFyKC0tMiksMTZweCAxNHB4IHZhcigtLTIpLDE3cHggMTRweCB2YXIoLS0yKSwxOHB4IDE0cHggdmFyKC0tMyksMTlweCAxNHB4IHZhcigtLTQpLDIwcHggMTRweCB2YXIoLS0wKSwyMXB4IDE0cHggdmFyKC0tMCksMjJweCAxNHB4IHZhcigtLTApLDIzcHggMTRweCB2YXIoLS0wKSwwcHggMTVweCB2YXIoLS0wKSwxcHggMTVweCB2YXIoLS0wKSwycHggMTVweCB2YXIoLS0wKSwzcHggMTVweCB2YXIoLS0xKSw0cHggMTVweCB2YXIoLS0yKSw1cHggMTVweCB2YXIoLS0yKSw2cHggMTVweCB2YXIoLS0yKSw3cHggMTVweCB2YXIoLS0yKSw4cHggMTVweCB2YXIoLS0yKSw5cHggMTVweCB2YXIoLS0yKSwxMHB4IDE1cHggdmFyKC0tMiksMTFweCAxNXB4IHZhcigtLTIpLDEycHggMTVweCB2YXIoLS0yKSwxM3B4IDE1cHggdmFyKC0tMiksMTRweCAxNXB4IHZhcigtLTIpLDE1cHggMTVweCB2YXIoLS0yKSwxNnB4IDE1cHggdmFyKC0tMiksMTdweCAxNXB4IHZhcigtLTIpLDE4cHggMTVweCB2YXIoLS0zKSwxOXB4IDE1cHggdmFyKC0tMSksMjBweCAxNXB4IHZhcigtLTApLDIxcHggMTVweCB2YXIoLS0wKSwyMnB4IDE1cHggdmFyKC0tMCksMjNweCAxNXB4IHZhcigtLTApLDBweCAxNnB4IHZhcigtLTApLDFweCAxNnB4IHZhcigtLTApLDJweCAxNnB4IHZhcigtLTApLDNweCAxNnB4IHZhcigtLTQpLDRweCAxNnB4IHZhcigtLTIpLDVweCAxNnB4IHZhcigtLTIpLDZweCAxNnB4IHZhcigtLTIpLDdweCAxNnB4IHZhcigtLTIpLDhweCAxNnB4IHZhcigtLTIpLDlweCAxNnB4IHZhcigtLTIpLDEwcHggMTZweCB2YXIoLS0yKSwxMXB4IDE2cHggdmFyKC0tMiksMTJweCAxNnB4IHZhcigtLTIpLDEzcHggMTZweCB2YXIoLS0yKSwxNHB4IDE2cHggdmFyKC0tMiksMTVweCAxNnB4IHZhcigtLTIpLDE2cHggMTZweCB2YXIoLS0yKSwxN3B4IDE2cHggdmFyKC0tMiksMThweCAxNnB4IHZhcigtLTEpLDE5cHggMTZweCB2YXIoLS0wKSwyMHB4IDE2cHggdmFyKC0tMCksMjFweCAxNnB4IHZhcigtLTApLDIycHggMTZweCB2YXIoLS0wKSwyM3B4IDE2cHggdmFyKC0tMCksMHB4IDE3cHggdmFyKC0tMCksMXB4IDE3cHggdmFyKC0tMCksMnB4IDE3cHggdmFyKC0tMCksM3B4IDE3cHggdmFyKC0tNCksNHB4IDE3cHggdmFyKC0tMiksNXB4IDE3cHggdmFyKC0tMiksNnB4IDE3cHggdmFyKC0tMiksN3B4IDE3cHggdmFyKC0tMiksOHB4IDE3cHggdmFyKC0tMiksOXB4IDE3cHggdmFyKC0tMiksMTBweCAxN3B4IHZhcigtLTIpLDExcHggMTdweCB2YXIoLS0yKSwxMnB4IDE3cHggdmFyKC0tMiksMTNweCAxN3B4IHZhcigtLTIpLDE0cHggMTdweCB2YXIoLS0yKSwxNXB4IDE3cHggdmFyKC0tMiksMTZweCAxN3B4IHZhcigtLTIpLDE3cHggMTdweCB2YXIoLS0yKSwxOHB4IDE3cHggdmFyKC0tMyksMTlweCAxN3B4IHZhcigtLTEpLDIwcHggMTdweCB2YXIoLS0wKSwyMXB4IDE3cHggdmFyKC0tMCksMjJweCAxN3B4IHZhcigtLTApLDIzcHggMTdweCB2YXIoLS0wKSwwcHggMThweCB2YXIoLS0wKSwxcHggMThweCB2YXIoLS0wKSwycHggMThweCB2YXIoLS0wKSwzcHggMThweCB2YXIoLS00KSw0cHggMThweCB2YXIoLS0yKSw1cHggMThweCB2YXIoLS0yKSw2cHggMThweCB2YXIoLS0yKSw3cHggMThweCB2YXIoLS0yKSw4cHggMThweCB2YXIoLS0yKSw5cHggMThweCB2YXIoLS0yKSwxMHB4IDE4cHggdmFyKC0tMiksMTFweCAxOHB4IHZhcigtLTIpLDEycHggMThweCB2YXIoLS0yKSwxM3B4IDE4cHggdmFyKC0tMiksMTRweCAxOHB4IHZhcigtLTIpLDE1cHggMThweCB2YXIoLS0yKSwxNnB4IDE4cHggdmFyKC0tMiksMTdweCAxOHB4IHZhcigtLTIpLDE4cHggMThweCB2YXIoLS0zKSwxOXB4IDE4cHggdmFyKC0tMSksMjBweCAxOHB4IHZhcigtLTApLDIxcHggMThweCB2YXIoLS0wKSwyMnB4IDE4cHggdmFyKC0tMCksMjNweCAxOHB4IHZhcigtLTApLDBweCAxOXB4IHZhcigtLTApLDFweCAxOXB4IHZhcigtLTApLDJweCAxOXB4IHZhcigtLTApLDNweCAxOXB4IHZhcigtLTQpLDRweCAxOXB4IHZhcigtLTIpLDVweCAxOXB4IHZhcigtLTIpLDZweCAxOXB4IHZhcigtLTIpLDdweCAxOXB4IHZhcigtLTIpLDhweCAxOXB4IHZhcigtLTIpLDlweCAxOXB4IHZhcigtLTIpLDEwcHggMTlweCB2YXIoLS0yKSwxMXB4IDE5cHggdmFyKC0tMiksMTJweCAxOXB4IHZhcigtLTIpLDEzcHggMTlweCB2YXIoLS0yKSwxNHB4IDE5cHggdmFyKC0tMiksMTVweCAxOXB4IHZhcigtLTIpLDE2cHggMTlweCB2YXIoLS0yKSwxN3B4IDE5cHggdmFyKC0tMyksMThweCAxOXB4IHZhcigtLTMpLDE5cHggMTlweCB2YXIoLS00KSwyMHB4IDE5cHggdmFyKC0tMCksMjFweCAxOXB4IHZhcigtLTApLDIycHggMTlweCB2YXIoLS0wKSwyM3B4IDE5cHggdmFyKC0tMCksMHB4IDIwcHggdmFyKC0tMCksMXB4IDIwcHggdmFyKC0tMCksMnB4IDIwcHggdmFyKC0tMCksM3B4IDIwcHggdmFyKC0tMSksNHB4IDIwcHggdmFyKC0tMSksNXB4IDIwcHggdmFyKC0tMSksNnB4IDIwcHggdmFyKC0tMSksN3B4IDIwcHggdmFyKC0tMSksOHB4IDIwcHggdmFyKC0tMSksOXB4IDIwcHggdmFyKC0tMSksMTBweCAyMHB4IHZhcigtLTEpLDExcHggMjBweCB2YXIoLS0xKSwxMnB4IDIwcHggdmFyKC0tMSksMTNweCAyMHB4IHZhcigtLTEpLDE0cHggMjBweCB2YXIoLS0xKSwxNXB4IDIwcHggdmFyKC0tMSksMTZweCAyMHB4IHZhcigtLTEpLDE3cHggMjBweCB2YXIoLS0xKSwxOHB4IDIwcHggdmFyKC0tMSksMTlweCAyMHB4IHZhcigtLTEpLDIwcHggMjBweCB2YXIoLS0wKSwyMXB4IDIwcHggdmFyKC0tMCksMjJweCAyMHB4IHZhcigtLTApLDIzcHggMjBweCB2YXIoLS0wKSwwcHggMjFweCB2YXIoLS0wKSwxcHggMjFweCB2YXIoLS0wKSwycHggMjFweCB2YXIoLS0xKSwzcHggMjFweCB2YXIoLS0yKSw0cHggMjFweCB2YXIoLS0yKSw1cHggMjFweCB2YXIoLS0yKSw2cHggMjFweCB2YXIoLS0yKSw3cHggMjFweCB2YXIoLS0yKSw4cHggMjFweCB2YXIoLS0yKSw5cHggMjFweCB2YXIoLS0yKSwxMHB4IDIxcHggdmFyKC0tMiksMTFweCAyMXB4IHZhcigtLTIpLDEycHggMjFweCB2YXIoLS0yKSwxM3B4IDIxcHggdmFyKC0tMiksMTRweCAyMXB4IHZhcigtLTIpLDE1cHggMjFweCB2YXIoLS0yKSwxNnB4IDIxcHggdmFyKC0tMiksMTdweCAyMXB4IHZhcigtLTIpLDE4cHggMjFweCB2YXIoLS0yKSwxOXB4IDIxcHggdmFyKC0tMiksMjBweCAyMXB4IHZhcigtLTEpLDIxcHggMjFweCB2YXIoLS0wKSwyMnB4IDIxcHggdmFyKC0tMCksMjNweCAyMXB4IHZhcigtLTApLDBweCAyMnB4IHZhcigtLTApLDFweCAyMnB4IHZhcigtLTApLDJweCAyMnB4IHZhcigtLTEpLDNweCAyMnB4IHZhcigtLTIpLDRweCAyMnB4IHZhcigtLTMpLDVweCAyMnB4IHZhcigtLTMpLDZweCAyMnB4IHZhcigtLTMpLDdweCAyMnB4IHZhcigtLTMpLDhweCAyMnB4IHZhcigtLTMpLDlweCAyMnB4IHZhcigtLTMpLDEwcHggMjJweCB2YXIoLS0zKSwxMXB4IDIycHggdmFyKC0tMyksMTJweCAyMnB4IHZhcigtLTMpLDEzcHggMjJweCB2YXIoLS0zKSwxNHB4IDIycHggdmFyKC0tMyksMTVweCAyMnB4IHZhcigtLTMpLDE2cHggMjJweCB2YXIoLS0zKSwxN3B4IDIycHggdmFyKC0tMyksMThweCAyMnB4IHZhcigtLTMpLDE5cHggMjJweCB2YXIoLS0yKSwyMHB4IDIycHggdmFyKC0tMSksMjFweCAyMnB4IHZhcigtLTApLDIycHggMjJweCB2YXIoLS0wKSwyM3B4IDIycHggdmFyKC0tMCksMHB4IDIzcHggdmFyKC0tMCksMXB4IDIzcHggdmFyKC0tMCksMnB4IDIzcHggdmFyKC0tMCksM3B4IDIzcHggdmFyKC0tMSksNHB4IDIzcHggdmFyKC0tMSksNXB4IDIzcHggdmFyKC0tMSksNnB4IDIzcHggdmFyKC0tMSksN3B4IDIzcHggdmFyKC0tMSksOHB4IDIzcHggdmFyKC0tMSksOXB4IDIzcHggdmFyKC0tMSksMTBweCAyM3B4IHZhcigtLTEpLDExcHggMjNweCB2YXIoLS0xKSwxMnB4IDIzcHggdmFyKC0tMSksMTNweCAyM3B4IHZhcigtLTEpLDE0cHggMjNweCB2YXIoLS0xKSwxNXB4IDIzcHggdmFyKC0tMSksMTZweCAyM3B4IHZhcigtLTEpLDE3cHggMjNweCB2YXIoLS0xKSwxOHB4IDIzcHggdmFyKC0tMSksMTlweCAyM3B4IHZhcigtLTEpLDIwcHggMjNweCB2YXIoLS0wKSwyMXB4IDIzcHggdmFyKC0tMCksMjJweCAyM3B4IHZhcigtLTApLDIzcHggMjNweCB2YXIoLS0wKX08L3N0eWxlPg
```

<details>
  <summary>html snippet for <code>Mirakai Scrolls</code></summary>

```html
<div />

<style>
div {
  --0: transparent;
  --1: #8b3615;
  --2: #d49443;
  --3: #c57032;
  --4: #76290c;
  width: 1px;
  height: 1px;
  background: var(--0);
  display: inline-block;
  transform: scale(10);
  transform-origin: top left;
  box-shadow: 
    0px 0px var(--0),
    1px 0px var(--0),
    2px 0px var(--0),
    3px 0px var(--1),
    4px 0px var(--1),
    5px 0px var(--1),
    6px 0px var(--1),
    7px 0px var(--1),
    8px 0px var(--1),
    9px 0px var(--1),
    10px 0px var(--1),
    11px 0px var(--1),
    12px 0px var(--1),
    13px 0px var(--1),
    14px 0px var(--1),
    15px 0px var(--1),
    16px 0px var(--1),
    17px 0px var(--1),
    18px 0px var(--1),
    19px 0px var(--1),
    20px 0px var(--0),
    21px 0px var(--0),
    22px 0px var(--0),
    23px 0px var(--0),
    0px 1px var(--0),
    1px 1px var(--0),
    2px 1px var(--1),
    3px 1px var(--2),
    4px 1px var(--2),
    5px 1px var(--2),
    6px 1px var(--2),
    7px 1px var(--2),
    8px 1px var(--2),
    9px 1px var(--2),
    10px 1px var(--2),
    11px 1px var(--2),
    12px 1px var(--2),
    13px 1px var(--2),
    14px 1px var(--2),
    15px 1px var(--2),
    16px 1px var(--2),
    17px 1px var(--2),
    18px 1px var(--2),
    19px 1px var(--2),
    20px 1px var(--1),
    21px 1px var(--0),
    22px 1px var(--0),
    23px 1px var(--0),
    0px 2px var(--0),
    1px 2px var(--0),
    2px 2px var(--1),
    3px 2px var(--2),
    4px 2px var(--3),
    5px 2px var(--3),
    6px 2px var(--3),
    7px 2px var(--3),
    8px 2px var(--3),
    9px 2px var(--3),
    10px 2px var(--3),
    11px 2px var(--3),
    12px 2px var(--3),
    13px 2px var(--3),
    14px 2px var(--3),
    15px 2px var(--3),
    16px 2px var(--3),
    17px 2px var(--3),
    18px 2px var(--3),
    19px 2px var(--2),
    20px 2px var(--1),
    21px 2px var(--0),
    22px 2px var(--0),
    23px 2px var(--0),
    0px 3px var(--0),
    1px 3px var(--0),
    2px 3px var(--0),
    3px 3px var(--1),
    4px 3px var(--1),
    5px 3px var(--1),
    6px 3px var(--1),
    7px 3px var(--1),
    8px 3px var(--1),
    9px 3px var(--1),
    10px 3px var(--1),
    11px 3px var(--1),
    12px 3px var(--1),
    13px 3px var(--1),
    14px 3px var(--1),
    15px 3px var(--1),
    16px 3px var(--1),
    17px 3px var(--1),
    18px 3px var(--1),
    19px 3px var(--1),
    20px 3px var(--0),
    21px 3px var(--0),
    22px 3px var(--0),
    23px 3px var(--0),
    0px 4px var(--0),
    1px 4px var(--0),
    2px 4px var(--0),
    3px 4px var(--4),
    4px 4px var(--3),
    5px 4px var(--3),
    6px 4px var(--3),
    7px 4px var(--3),
    8px 4px var(--3),
    9px 4px var(--3),
    10px 4px var(--3),
    11px 4px var(--3),
    12px 4px var(--3),
    13px 4px var(--3),
    14px 4px var(--3),
    15px 4px var(--3),
    16px 4px var(--3),
    17px 4px var(--3),
    18px 4px var(--3),
    19px 4px var(--4),
    20px 4px var(--0),
    21px 4px var(--0),
    22px 4px var(--0),
    23px 4px var(--0),
    0px 5px var(--0),
    1px 5px var(--0),
    2px 5px var(--0),
    3px 5px var(--4),
    4px 5px var(--2),
    5px 5px var(--2),
    6px 5px var(--2),
    7px 5px var(--2),
    8px 5px var(--2),
    9px 5px var(--2),
    10px 5px var(--2),
    11px 5px var(--2),
    12px 5px var(--2),
    13px 5px var(--2),
    14px 5px var(--2),
    15px 5px var(--2),
    16px 5px var(--2),
    17px 5px var(--2),
    18px 5px var(--3),
    19px 5px var(--4),
    20px 5px var(--0),
    21px 5px var(--0),
    22px 5px var(--0),
    23px 5px var(--0),
    0px 6px var(--0),
    1px 6px var(--0),
    2px 6px var(--0),
    3px 6px var(--4),
    4px 6px var(--2),
    5px 6px var(--2),
    6px 6px var(--2),
    7px 6px var(--2),
    8px 6px var(--2),
    9px 6px var(--2),
    10px 6px var(--2),
    11px 6px var(--2),
    12px 6px var(--2),
    13px 6px var(--2),
    14px 6px var(--2),
    15px 6px var(--2),
    16px 6px var(--2),
    17px 6px var(--2),
    18px 6px var(--3),
    19px 6px var(--4),
    20px 6px var(--0),
    21px 6px var(--0),
    22px 6px var(--0),
    23px 6px var(--0),
    0px 7px var(--0),
    1px 7px var(--0),
    2px 7px var(--0),
    3px 7px var(--1),
    4px 7px var(--2),
    5px 7px var(--2),
    6px 7px var(--2),
    7px 7px var(--2),
    8px 7px var(--2),
    9px 7px var(--2),
    10px 7px var(--2),
    11px 7px var(--2),
    12px 7px var(--2),
    13px 7px var(--2),
    14px 7px var(--2),
    15px 7px var(--2),
    16px 7px var(--2),
    17px 7px var(--2),
    18px 7px var(--3),
    19px 7px var(--4),
    20px 7px var(--0),
    21px 7px var(--0),
    22px 7px var(--0),
    23px 7px var(--0),
    0px 8px var(--0),
    1px 8px var(--0),
    2px 8px var(--0),
    3px 8px var(--4),
    4px 8px var(--2),
    5px 8px var(--2),
    6px 8px var(--2),
    7px 8px var(--2),
    8px 8px var(--2),
    9px 8px var(--2),
    10px 8px var(--2),
    11px 8px var(--2),
    12px 8px var(--2),
    13px 8px var(--2),
    14px 8px var(--2),
    15px 8px var(--2),
    16px 8px var(--2),
    17px 8px var(--2),
    18px 8px var(--3),
    19px 8px var(--4),
    20px 8px var(--0),
    21px 8px var(--0),
    22px 8px var(--0),
    23px 8px var(--0),
    0px 9px var(--0),
    1px 9px var(--0),
    2px 9px var(--0),
    3px 9px var(--1),
    4px 9px var(--2),
    5px 9px var(--2),
    6px 9px var(--2),
    7px 9px var(--2),
    8px 9px var(--2),
    9px 9px var(--2),
    10px 9px var(--2),
    11px 9px var(--2),
    12px 9px var(--2),
    13px 9px var(--2),
    14px 9px var(--2),
    15px 9px var(--2),
    16px 9px var(--2),
    17px 9px var(--2),
    18px 9px var(--3),
    19px 9px var(--4),
    20px 9px var(--0),
    21px 9px var(--0),
    22px 9px var(--0),
    23px 9px var(--0),
    0px 10px var(--0),
    1px 10px var(--0),
    2px 10px var(--0),
    3px 10px var(--1),
    4px 10px var(--2),
    5px 10px var(--2),
    6px 10px var(--2),
    7px 10px var(--2),
    8px 10px var(--2),
    9px 10px var(--2),
    10px 10px var(--2),
    11px 10px var(--2),
    12px 10px var(--2),
    13px 10px var(--2),
    14px 10px var(--2),
    15px 10px var(--2),
    16px 10px var(--2),
    17px 10px var(--2),
    18px 10px var(--3),
    19px 10px var(--4),
    20px 10px var(--0),
    21px 10px var(--0),
    22px 10px var(--0),
    23px 10px var(--0),
    0px 11px var(--0),
    1px 11px var(--0),
    2px 11px var(--0),
    3px 11px var(--0),
    4px 11px var(--1),
    5px 11px var(--2),
    6px 11px var(--2),
    7px 11px var(--2),
    8px 11px var(--2),
    9px 11px var(--2),
    10px 11px var(--2),
    11px 11px var(--2),
    12px 11px var(--2),
    13px 11px var(--2),
    14px 11px var(--2),
    15px 11px var(--2),
    16px 11px var(--2),
    17px 11px var(--2),
    18px 11px var(--3),
    19px 11px var(--4),
    20px 11px var(--0),
    21px 11px var(--0),
    22px 11px var(--0),
    23px 11px var(--0),
    0px 12px var(--0),
    1px 12px var(--0),
    2px 12px var(--0),
    3px 12px var(--1),
    4px 12px var(--2),
    5px 12px var(--2),
    6px 12px var(--2),
    7px 12px var(--2),
    8px 12px var(--2),
    9px 12px var(--2),
    10px 12px var(--2),
    11px 12px var(--2),
    12px 12px var(--2),
    13px 12px var(--2),
    14px 12px var(--2),
    15px 12px var(--2),
    16px 12px var(--2),
    17px 12px var(--2),
    18px 12px var(--3),
    19px 12px var(--4),
    20px 12px var(--0),
    21px 12px var(--0),
    22px 12px var(--0),
    23px 12px var(--0),
    0px 13px var(--0),
    1px 13px var(--0),
    2px 13px var(--0),
    3px 13px var(--1),
    4px 13px var(--2),
    5px 13px var(--2),
    6px 13px var(--2),
    7px 13px var(--2),
    8px 13px var(--2),
    9px 13px var(--2),
    10px 13px var(--2),
    11px 13px var(--2),
    12px 13px var(--2),
    13px 13px var(--2),
    14px 13px var(--2),
    15px 13px var(--2),
    16px 13px var(--2),
    17px 13px var(--2),
    18px 13px var(--3),
    19px 13px var(--1),
    20px 13px var(--0),
    21px 13px var(--0),
    22px 13px var(--0),
    23px 13px var(--0),
    0px 14px var(--0),
    1px 14px var(--0),
    2px 14px var(--0),
    3px 14px var(--4),
    4px 14px var(--2),
    5px 14px var(--2),
    6px 14px var(--2),
    7px 14px var(--2),
    8px 14px var(--2),
    9px 14px var(--2),
    10px 14px var(--2),
    11px 14px var(--2),
    12px 14px var(--2),
    13px 14px var(--2),
    14px 14px var(--2),
    15px 14px var(--2),
    16px 14px var(--2),
    17px 14px var(--2),
    18px 14px var(--3),
    19px 14px var(--4),
    20px 14px var(--0),
    21px 14px var(--0),
    22px 14px var(--0),
    23px 14px var(--0),
    0px 15px var(--0),
    1px 15px var(--0),
    2px 15px var(--0),
    3px 15px var(--1),
    4px 15px var(--2),
    5px 15px var(--2),
    6px 15px var(--2),
    7px 15px var(--2),
    8px 15px var(--2),
    9px 15px var(--2),
    10px 15px var(--2),
    11px 15px var(--2),
    12px 15px var(--2),
    13px 15px var(--2),
    14px 15px var(--2),
    15px 15px var(--2),
    16px 15px var(--2),
    17px 15px var(--2),
    18px 15px var(--3),
    19px 15px var(--1),
    20px 15px var(--0),
    21px 15px var(--0),
    22px 15px var(--0),
    23px 15px var(--0),
    0px 16px var(--0),
    1px 16px var(--0),
    2px 16px var(--0),
    3px 16px var(--4),
    4px 16px var(--2),
    5px 16px var(--2),
    6px 16px var(--2),
    7px 16px var(--2),
    8px 16px var(--2),
    9px 16px var(--2),
    10px 16px var(--2),
    11px 16px var(--2),
    12px 16px var(--2),
    13px 16px var(--2),
    14px 16px var(--2),
    15px 16px var(--2),
    16px 16px var(--2),
    17px 16px var(--2),
    18px 16px var(--1),
    19px 16px var(--0),
    20px 16px var(--0),
    21px 16px var(--0),
    22px 16px var(--0),
    23px 16px var(--0),
    0px 17px var(--0),
    1px 17px var(--0),
    2px 17px var(--0),
    3px 17px var(--4),
    4px 17px var(--2),
    5px 17px var(--2),
    6px 17px var(--2),
    7px 17px var(--2),
    8px 17px var(--2),
    9px 17px var(--2),
    10px 17px var(--2),
    11px 17px var(--2),
    12px 17px var(--2),
    13px 17px var(--2),
    14px 17px var(--2),
    15px 17px var(--2),
    16px 17px var(--2),
    17px 17px var(--2),
    18px 17px var(--3),
    19px 17px var(--1),
    20px 17px var(--0),
    21px 17px var(--0),
    22px 17px var(--0),
    23px 17px var(--0),
    0px 18px var(--0),
    1px 18px var(--0),
    2px 18px var(--0),
    3px 18px var(--4),
    4px 18px var(--2),
    5px 18px var(--2),
    6px 18px var(--2),
    7px 18px var(--2),
    8px 18px var(--2),
    9px 18px var(--2),
    10px 18px var(--2),
    11px 18px var(--2),
    12px 18px var(--2),
    13px 18px var(--2),
    14px 18px var(--2),
    15px 18px var(--2),
    16px 18px var(--2),
    17px 18px var(--2),
    18px 18px var(--3),
    19px 18px var(--1),
    20px 18px var(--0),
    21px 18px var(--0),
    22px 18px var(--0),
    23px 18px var(--0),
    0px 19px var(--0),
    1px 19px var(--0),
    2px 19px var(--0),
    3px 19px var(--4),
    4px 19px var(--2),
    5px 19px var(--2),
    6px 19px var(--2),
    7px 19px var(--2),
    8px 19px var(--2),
    9px 19px var(--2),
    10px 19px var(--2),
    11px 19px var(--2),
    12px 19px var(--2),
    13px 19px var(--2),
    14px 19px var(--2),
    15px 19px var(--2),
    16px 19px var(--2),
    17px 19px var(--3),
    18px 19px var(--3),
    19px 19px var(--4),
    20px 19px var(--0),
    21px 19px var(--0),
    22px 19px var(--0),
    23px 19px var(--0),
    0px 20px var(--0),
    1px 20px var(--0),
    2px 20px var(--0),
    3px 20px var(--1),
    4px 20px var(--1),
    5px 20px var(--1),
    6px 20px var(--1),
    7px 20px var(--1),
    8px 20px var(--1),
    9px 20px var(--1),
    10px 20px var(--1),
    11px 20px var(--1),
    12px 20px var(--1),
    13px 20px var(--1),
    14px 20px var(--1),
    15px 20px var(--1),
    16px 20px var(--1),
    17px 20px var(--1),
    18px 20px var(--1),
    19px 20px var(--1),
    20px 20px var(--0),
    21px 20px var(--0),
    22px 20px var(--0),
    23px 20px var(--0),
    0px 21px var(--0),
    1px 21px var(--0),
    2px 21px var(--1),
    3px 21px var(--2),
    4px 21px var(--2),
    5px 21px var(--2),
    6px 21px var(--2),
    7px 21px var(--2),
    8px 21px var(--2),
    9px 21px var(--2),
    10px 21px var(--2),
    11px 21px var(--2),
    12px 21px var(--2),
    13px 21px var(--2),
    14px 21px var(--2),
    15px 21px var(--2),
    16px 21px var(--2),
    17px 21px var(--2),
    18px 21px var(--2),
    19px 21px var(--2),
    20px 21px var(--1),
    21px 21px var(--0),
    22px 21px var(--0),
    23px 21px var(--0),
    0px 22px var(--0),
    1px 22px var(--0),
    2px 22px var(--1),
    3px 22px var(--2),
    4px 22px var(--3),
    5px 22px var(--3),
    6px 22px var(--3),
    7px 22px var(--3),
    8px 22px var(--3),
    9px 22px var(--3),
    10px 22px var(--3),
    11px 22px var(--3),
    12px 22px var(--3),
    13px 22px var(--3),
    14px 22px var(--3),
    15px 22px var(--3),
    16px 22px var(--3),
    17px 22px var(--3),
    18px 22px var(--3),
    19px 22px var(--2),
    20px 22px var(--1),
    21px 22px var(--0),
    22px 22px var(--0),
    23px 22px var(--0),
    0px 23px var(--0),
    1px 23px var(--0),
    2px 23px var(--0),
    3px 23px var(--1),
    4px 23px var(--1),
    5px 23px var(--1),
    6px 23px var(--1),
    7px 23px var(--1),
    8px 23px var(--1),
    9px 23px var(--1),
    10px 23px var(--1),
    11px 23px var(--1),
    12px 23px var(--1),
    13px 23px var(--1),
    14px 23px var(--1),
    15px 23px var(--1),
    16px 23px var(--1),
    17px 23px var(--1),
    18px 23px var(--1),
    19px 23px var(--1),
    20px 23px var(--0),
    21px 23px var(--0),
    22px 23px var(--0),
    23px 23px var(--0)
}
</style>
```
</details>
