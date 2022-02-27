<template>
  <div id="app">
  </div>
</template>

<script>
import addOAuthInterceptor from 'axios-oauth-1.0a';
export default {
  data() {
    return{
      Twitter: require('twitter')
    }
  },
  mounted(){
    //fetchがcorsで弾かれるから使えない。
    // const {TwitterApi} = require('twitter-api-v2');
    // const client = new TwitterApi({
    //   appKey: '3VSihSK08fjwlsLIodkGnGSN1',
    //   appSecret: 'WyBxs6soFfjVDXInUemB9aqU5TSPb4agP8m4Km5lgkfQ4uesPy',
    //   accessToken: '1494570525166690304-bBOeJQBl5nq6XLMaaKAOxLf0W2S7bi',
    //   accessSecret: 'EcsKuV4GPoh7rTg4T1XaCLiRKUQjGyck66MZpBjPWhcwN'
    // });
    // client.v1.tweet('This tweet was written by a bot').then((val) => {
    //     console.log(val)
    //     console.log("success")
    // }).catch((err) => {
    //     console.log(err)
    // })


    //今考えればtwitter aoi1.1の申請をしてなかったからかもしれないが、corsに引っかかって使えない側面もある。axiosだから抜けれると思ってたけど、何故か弾かれた。
    // const api = this.$axios.create({
    //   baseURL: 'https://api.twitter.com/1.1',
    //   headers: {
    //     // Authorization was copied from the Postman headers
    //     Authorization: 'OAuth oauth_consumer_key="3VSihSK08fjwlsLIodkGnGSN1",oauth_consumer_secret="WyBxs6soFfjVDXInUemB9aqU5TSPb4agP8m4Km5lgkfQ4uesPy",oauth_token="1494570525166690304-bBOeJQBl5nq6XLMaaKAOxLf0W2S7bi",oauth_token_secret="",,oauth_signature_method="H,MAC-SHA1",,oauth_timestamp="1,515677408",,oauth_nonce="nonce",,oauth_version="1,.0",,oauth_signature="signature"',
    //   },
    //   withCredentials: true,
    // });

    // const getListStatuses = (owner_screen_name, slug) => {
    //   return api.get('/lists/statuses.json', { owner_screen_name, slug });
    // };
    // console.log(getListStatuses)



    //今の所一番可能性があるんじゃないかと思っている。oauth-i.0aを使用する。getはできる。
    const OAuth = require('oauth-1.0a')
    const crypto = require('crypto')
    const oauth = OAuth({
      consumer: {
        key: '3VSihSK08fjwlsLIodkGnGSN1',
        secret: 'WyBxs6soFfjVDXInUemB9aqU5TSPb4agP8m4Km5lgkfQ4uesPy'
      },
      signature_method: 'HMAC-SHA1',
      hash_function(base_string, key) {
        return crypto
          .createHmac('sha1', key)
          .update(base_string)
          .digest('base64')
      },
    })
    const token = {
      key: '1494570525166690304-bBOeJQBl5nq6XLMaaKAOxLf0W2S7bi',
      secret: 'EcsKuV4GPoh7rTg4T1XaCLiRKUQjGyck66MZpBjPWhcwN',
    }
    const request = {
      url: "https://api.twitter.com/2/users/1494570525166690304/tweets",
      method: 'GET',
    }
    let config = {
      url: request.url,
      method: request.method,
      headers: oauth.toHeader(oauth.authorize(request, token))
    }
    console.log(config.headers)

    this.$axios.$get(
      "/api/users/1494570525166690304/tweets", config
    )
      .then(function (response) {
        console.log(response)
      })
      .catch(function (error) {
        console.log(error)
      })

    //postは無理
    // const oauth = OAuth({
    //   consumer: {
    //     key: '3VSihSK08fjwlsLIodkGnGSN1',
    //     secret: 'WyBxs6soFfjVDXInUemB9aqU5TSPb4agP8m4Km5lgkfQ4uesPy'
    //   },
    //   signature_method: 'HMAC-SHA1',
    //   hash_function(base_string, key) {
    //     return crypto
    //       .createHmac('sha1', key)
    //       .update(base_string)
    //       .digest('base64')
    //   },
    // })
    // const token = {
    //   key: '1494570525166690304-bBOeJQBl5nq6XLMaaKAOxLf0W2S7bi',
    //   secret: 'EcsKuV4GPoh7rTg4T1XaCLiRKUQjGyck66MZpBjPWhcwN',
    // }
    // const request = {
    //   url: "https://api.twitter.com/2/tweets?status=Hello%20world",
    //   method: 'POST',
    // }
    // let config = {
    //   url: request.url,
    //   method: request.method,
    //   headers: oauth.toHeader(oauth.authorize(request, token))
    // }
    // console.log(config.headers)

    // this.$axios.$post(
    //   "/api/tweets?status=Hello%20world", config
    // )
    //   .then(function (response) {
    //     console.log(response)
    //   })
    //   .catch(function (error) {
    //     console.log(error)
    //   })
  }
}
</script>
