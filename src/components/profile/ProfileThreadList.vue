<template>
    <div class="profilethreadlist container">
        <div class="grey darken-3 card-panel threads">
          <router-link :to="{name: 'CreateThread'}"><button class="btn grey darken-2">Create Thread</button></router-link>
          <table>
            <tr>
              <th class="white-text">Thread</th>
              <th class="white-text">Author</th>
              <th class="white-text center">Replies</th>
              <th class="white-text center">Views</th>
              <th class="white-text center">Date Created</th>
            </tr>
            <tr v-for="thread in threads" :key="thread.id">
              <td class="white-text"><router-link :to="{name: 'ViewThread', params: {id: thread.id}}">{{ thread.title }}</router-link></td>
              <td class="white-text"><router-link :to="{name: 'ViewProfile', params: {slug: thread.slug}}">{{ thread.author }}</router-link></td>
              <td class="white-text center">{{ thread.replies }}</td>
              <td class="white-text center">{{ thread.views }}</td>
              <td class="white-text center">{{ thread.time }}</td>
            </tr>
          </table>
        </div>  
    </div>
</template>

<script>
import moment from 'moment'
import db from '@/firebase/init'
import firebase from 'firebase'
export default {
    name: 'ProfileThreadList',
    data() {
        return{
            threads: [],
        }
    },
    created(){
        let userRef = db.collection('users').doc(this.$route.params.slug)

        userRef.get().then(doc => {
            doc.data().threadList.forEach(element => {
                let ref = db.collection('threads').doc(''+element)

                ref.get().then(doc => {
                    this.threads.push({
                        time: moment(doc.data().time).format('MMMM Do YYYY, h:mm:ss a'),
                        author: doc.data().author,
                        replies: doc.data().replies,
                        title: doc.data().title,
                        views: doc.data().views,
                        id: doc.data().id,
                        slug: doc.data().slug
                    })
                })
            });
        })
    }
}
</script>

<style scoped>

</style>