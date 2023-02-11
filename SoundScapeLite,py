### Iniicio
import streamlit as st
import spotipy
from spotipy.oauth2 import SpotifyClientCredentials

SPOTIPY_CLIENT_ID='d41fea1911754fca8e4d58664994b153'
SPOTIPY_CLIENT_SECRET='1f712317e2fa4b319200bcf6ad7a48db'

auth_manager = SpotifyClientCredentials(client_id=SPOTIPY_CLIENT_ID, client_secret=SPOTIPY_CLIENT_SECRET)
sp = spotipy.Spotify(auth_manager=auth_manager)

## Inicio

st.header(':blue[SoundScape]')
search_keyword = st.text_input(':blue[Give us your favorite song and we will find you the next one]')
search_results = []
tracks = []

if search_keyword is not None and len(str(search_keyword)) > 0:
  tracks = sp.search(q='track:'+ search_keyword,type='track', limit=21)
  tracks_list = tracks['tracks']['items']
  if len(tracks_list) > 0:
      for track in tracks_list:
        search_results.append(track['name'] + " - By - " + track['artists'][0]['name'])
  
selected_track = None
selected_track = st.selectbox("", search_results)

  
if selected_track is not None and len(tracks) > 0:
    tracks_list = tracks['tracks']['items']
    track_id = None
    if len(tracks_list) > 0:
        for track in tracks_list:
            str_temp = track['name'] + " - By - " + track['artists'][0]['name']
            if str_temp == selected_track:
                track_id = track['id']

    selected_track_choice = None
    
    time_signature = 1
    mode = 1
    key =1
    
    if track_id is not None:
        id = track_id
        pop = round(sp.track(id)['popularity'])
        #mode = round(sp.track(id)['mode'])
        #key = round(sp.track(id)['key'])
        #time_signature = round(sp.track(id)['time_signature'])
        acousticness = round(sp.audio_features(id)[0]['acousticness']*100)
        duration = sp.track(id)['duration_ms']/60000
        energy = round(sp.audio_features(id)[0]['energy']*100)
        dance = round(sp.audio_features(id)[0]['danceability']*100)
        valence = round(sp.audio_features(id)[0]['valence']*100)
        preview_url_1 = sp.track(id)['preview_url']
        st.audio(preview_url_1, format="audio/mp3")

        st.write(':blue[Stats]',':blue[{]','Pop : ',pop,'Acoustic : ',acousticness,'Duration : ',duration,'Energy : ',energy,'Dance : ',dance,'   Valence : ',valence,':blue[}]')
        st.write(':blue[------------------------ This is for you ------------------------]')
        st.sidebar.write(':blue[Make it better]')
        
        with st.container():
          popT = st.sidebar.number_input('Pop [0-100]', min_value=0,max_value=100,value=pop)
          acousticnessT = st.sidebar.number_input('Acoustic [0-100]', min_value=-10, max_value=100,value=acousticness)
          #durationT = st.sidebar.number_input('Duration [0-100]', min_value=0, max_value=999999999,value=duration)
          energyT = st.sidebar.number_input('Energy [0-100]', min_value=0,max_value=100,value=energy)
          moodT = st.sidebar.number_input('Mood [0-100]', min_value=0, max_value=100,value=valence)
          danceT = st.sidebar.number_input('Dance [0-100]', min_value=0,max_value=100,value=dance)

          
        moodS =moodT/100
        acousticnessS = acousticnessT/100
        #durationS = durationT * 60000
        energyS = energyT/100
        danceS = danceT/100
        
        play_tracks = sp.recommendations(seed_tracks=[track_id],limit=10,target_valence = moodS, target_popularity = pop,target_acousticness=acousticnessS,target_energy=energyS,target_danceability=danceS)
        t = len(play_tracks['tracks'])
        for i in range(t):
          name = play_tracks['tracks'][i]['name']
          uri = play_tracks['tracks'][i]['uri']
          url = play_tracks['tracks'][i]['external_urls']['spotify']
          arti = play_tracks['tracks'][i]['artists'][0]['name']  
          pop = round(play_tracks['tracks'][i]['popularity'])
          id = play_tracks['tracks'][i]['id']
          acousticness = round(sp.audio_features(id)[0]['acousticness']*100)
          duration = sp.track(id)['duration_ms']/60000
          energy = round(sp.audio_features(id)[0]['energy']*100)
          dance = round(sp.audio_features(id)[0]['danceability']*100)
          valence = round(sp.audio_features(id)[0]['valence']*100)
          preview_url = play_tracks['tracks'][i]['preview_url']  
          with st.container():
            st.write(':blue[Name |]', name,':blue[ |]')
            st.write(':blue[Artist |]', arti,':blue[ |]',':blue[Sp.link <]', uri,':blue[>]',':blue[ |]',':blue[Url <]', url,':blue[>]')
            st.write(':blue[Stats]',':blue[{]','Pop : ',pop,'Acoustic : ',acousticness,'Duration : ',duration,'Energy : ',energy,'Dance : ',dance,'   Valence : ',valence,':blue[}]')
            if preview_url is not None: 
              st.audio(preview_url, format="audio/mp3")
            st.write("---------------------------------")
