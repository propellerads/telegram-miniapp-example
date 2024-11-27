# Example of miniapp for Telegram with Rewarded Interstitial

You can view the entire code of a small one-armed bandit application in `index.html`.

Rewarded Interstitial is implemented in it very simply:

First, there is an ad tag inside the `<head>` tag:

```html
<script src="//psaunsooksuthi.com/vignette.min.js" data-zone="8257405" data-sdk="show_8257405"></script>
```

Second, at the very end is the code that uses the advertising SDK:

```js
  const button = document.body.appendChild(document.createElement('button'))

  button.classList.add('watch_ads')
  button.innerText = 'Watch Ads (+15🪙)'

  const refresh = () => {
    // button is inactive until the ad is loaded
    button.setAttribute('disabled', 'disabled')

    show_8257405('preload').then(() => {
      // the ad is ready, the button is active
      button.removeAttribute('disabled')
    }).catch(e => console.error(e))
  }

  // start ad preloading
  refresh()

  button.addEventListener('click', () => {
    show_8257405().then(() => {
      // add 15 coins after the user has watched an ad
      updateBalanceByOne(15)

      // preloading ad again
      refresh()
    }).catch(e => {
      console.error(e)
      alert('Sorry, no ads this time!')
    })
  })
```

What this code does:

1. Creates a “Watch Ads” button on the page
2. Using the refresh function, it makes this button inactive and starts the ad preloading mechanism: `show_8257405('preload')`
3. `show_8257405('preload')` returns a Promise which will resolve when the ad is ready to be shown. In this case, the “Watch Ads” button becomes active.
4. When the button is clicked, the `show_8257405()` function is called, which shows the user an ad. This function also returns Promise that will resolve after the user sees the ad.
5. Since `show_8257405()` returns Promise, code is added in the then method that rewards the user for viewing the ad by awarding 15 coins and restarting the ad preloading mechanism.
