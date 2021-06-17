<script>
  import copy from "../data/doc.json";

  let url = "https://pudding.cool";
</script>

<svelte:head>
  <title>{copy.meta[0].headline}</title>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
  <meta name="description" content="{copy.meta[0].subhed}" />
  <meta name="author" content="" />
  <meta name="news_keywords" content="" />

  <meta property="og:title" content="{copy.meta[0].headline}" />
  <meta property="og:site_name" content="The Pudding" />
  <meta property="og:url" content="{url}" />
  <meta property="og:description" content="{copy.meta[0].subhed}" />
  <meta property="og:type" content="article" />
  <meta property="og:locale" content="en_US" />

  <meta property="og:image" content="{url}/assets/social/facebook.jpg" />
  <meta property="og:image:type" content="image/jpeg" />
  <meta property="og:image:width" content="1200" />
  <meta property="og:image:height" content="600" />

  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:site" content="{url}" />
  <meta name="twitter:creator" content="@puddingviz" />
  <meta name="twitter:title" content="{copy.meta[0].headline}" />
  <meta name="twitter:description" content="{copy.meta[0].subhed}" />
  <meta name="twitter:image:src" content="{url}/assets/social/twitter.jpg" />

  <meta name="robots" content="max-image-preview:large" />

  <link rel="canonical" href="{url}/" />
</svelte:head>
