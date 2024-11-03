
# Spotify API Project

Este projeto usa a [API do Spotify](https://developer.spotify.com/documentation/web-api/) para acessar informações sobre músicas, playlists, artistas e muito mais. Com ele, você poderá autenticar-se na API e realizar buscas e requisições personalizadas.

## Pré-requisitos

1. **Conta do Spotify**: para usar a API, você precisará de uma conta no Spotify.
2. **Spotify Developer Account**: registre-se em [Spotify for Developers](https://developer.spotify.com/).
3. **Client ID e Client Secret**: obtenha essas credenciais criando um novo projeto no [Spotify Developer Dashboard](https://developer.spotify.com/dashboard/applications).

## Instalação

1. Clone o repositório:

    ```bash
    git clone https://github.com/seuusuario/spotify-api-project.git
    cd spotify-api-project
    ```

2. Instale as dependências (exemplo para projetos em Python):

    ```bash
    pip install -r requirements.txt
    ```

3. Crie um arquivo `.env` na raiz do projeto para armazenar suas credenciais do Spotify:

    ```env
    SPOTIFY_CLIENT_ID=seu_client_id
    SPOTIFY_CLIENT_SECRET=seu_client_secret
    SPOTIFY_REDIRECT_URI=http://localhost:8888/callback
    ```

## Autenticação

A API do Spotify requer autenticação OAuth 2.0. Para realizar a autenticação:

1. Redirecione o usuário para o URL de autorização do Spotify:

    ```python
    import requests
    import urllib.parse

    auth_url = 'https://accounts.spotify.com/authorize'
    params = {
        'client_id': 'SEU_CLIENT_ID',
        'response_type': 'code',
        'redirect_uri': 'http://localhost:8888/callback',
        'scope': 'user-read-private user-read-email'  # altere os escopos conforme necessário
    }
    url = f"{auth_url}?{urllib.parse.urlencode(params)}"
    print(f"Acesse o URL de autorização: {url}")
    ```

2. Após a autorização, você receberá um `code` no callback. Use-o para solicitar um `access_token`:

    ```python
    token_url = 'https://accounts.spotify.com/api/token'
    auth_response = requests.post(token_url, data={
        'grant_type': 'authorization_code',
        'code': 'SEU_CODIGO',
        'redirect_uri': 'http://localhost:8888/callback',
        'client_id': 'SEU_CLIENT_ID',
        'client_secret': 'SEU_CLIENT_SECRET'
    })

    access_token = auth_response.json().get('access_token')
    ```

## Uso da API

Agora que você possui o `access_token`, poderá fazer chamadas para a API do Spotify. Veja alguns exemplos:

1. **Buscar um artista**:

    ```python
    search_url = "https://api.spotify.com/v1/search"
    headers = {
        'Authorization': f'Bearer {access_token}'
    }
    params = {
        'q': 'nome_do_artista',
        'type': 'artist'
    }

    response = requests.get(search_url, headers=headers, params=params)
    data = response.json()
    print(data)
    ```

2. **Obter informações de um álbum**:

    ```python
    album_id = "id_do_album"
    album_url = f"https://api.spotify.com/v1/albums/{album_id}"

    response = requests.get(album_url, headers=headers)
    album_info = response.json()
    print(album_info)
    ```

3. **Criar uma playlist (exemplo)**:

    ```python
    user_id = "seu_usuario"
    playlist_url = f"https://api.spotify.com/v1/users/{user_id}/playlists"

    payload = {
        "name": "Minha nova playlist",
        "description": "Playlist criada via API do Spotify",
        "public": False
    }

    response = requests.post(playlist_url, headers=headers, json=payload)
    playlist = response.json()
    print(playlist)
    ```

## Contribuição

1. Faça um fork do projeto.
2. Crie um novo branch para a feature (`git checkout -b feature/nova-feature`).
3. Commit suas mudanças (`git commit -m 'Adiciona nova feature'`).
4. Push para o branch (`git push origin feature/nova-feature`).
5. Abra um Pull Request.

## Licença

Este projeto está sob a licença MIT.

---


