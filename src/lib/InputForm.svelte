<script>
  const API_URL = "https://www.googleapis.com/youtube/v3/search";
  const channelIdCache = {}; // :Record<ChannelName, ChannelId>
  const channelVideos = {};

  let channelVidCount = 0;

  const getChannelId = async (channelName, key) => {
    if (channelIdCache[channelName]) return channelIdCache[channelName];

    const response = await fetch(
      `${API_URL}?key=${key}&type=channel&q=${channelName}`,
      {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json",
        },
      }
    );
    const data = await response.json();
    channelIdCache[channelName] = data.items[0].id.channelId;
    return channelIdCache[channelName];
  };

  const getChannelVids = async (channelId, key) => {
    const response = await fetch(
      `${API_URL}?key=${key}&channelId=${channelId}&part=snippet,id&order=date&type=video`
    );
    const data = await response.json();
    channelVidCount = data.pageInfo.totalResults;

    return data;
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    channelVidCount = 0;

    const channelName = e.target.channelName.value;
    const key = e.target.apiKey.value;
    const channelId = await getChannelId(channelName, key);
    getChannelVids(channelId, key);
  };
</script>

<form id="main-form" class="card" on:submit={handleSubmit}>
  <fieldset>
    <input type="text" name="apiKey" placeholder="&nbsp;" />
    <legend>YouTube API Key</legend>
  </fieldset>
  <fieldset>
    <input type="text" name="channelName" placeholder="&nbsp;" />
    <legend>YouTube Channel Name</legend>
  </fieldset>

  <button type="submit">Submit</button>
</form>

<output for="main-form">
  {channelVidCount} videos found.
</output>

<style>
  .card {
    border: 2px solid var(--c-offblack);
    border-radius: 0.25rem;
    margin-bottom: 2rem;
    padding: 1rem;

    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  fieldset {
    border: 2px solid var(--c-offblack);
    border-radius: 0.25rem;
    box-sizing: border-box;

    padding: 0;
    position: relative;

    height: 100%;
    width: 100%;
  }

  fieldset > legend {
    background-color: var(--c-white);
    left: 0.25rem;
    line-height: 1;
    pointer-events: none;
    position: absolute;
    top: 0.25rem;
    transition: top 0.2s ease-in-out;
    transition-property: top, color, font-size;
  }

  fieldset > input {
    background: none;
    border: none;
    box-sizing: border-box;
    margin: 0;
    padding: 0.25rem 0.5rem;

    height: 100%;
    width: 100%;
  }

  fieldset > input:where(:focus, :focus-within) + legend {
    top: -0.625rem;
  }

  fieldset > input:not(:placeholder-shown) + legend {
    color: #666;
    font-size: 0.8rem;
    top: -0.625rem;
  }

  button[type="submit"] {
    background-color: var(--c-red);
    color: var(--c-white);
  }
</style>
