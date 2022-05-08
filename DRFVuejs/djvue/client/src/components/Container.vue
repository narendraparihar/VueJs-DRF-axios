<template>
  
  <div class="row">
       <div class="col-lg-6">
          <input type="hidden" v-model="url">
            <input type="text" class="form-control mt-2" placeholder="Title" v-model="title">
            <input type="text" class="form-control mt-2" placeholder="Body" v-model="body">
            <button class="btn btn-block btn-success" @click="postBlog">Post</button>
       </div>
       <div class="col-lg-6">
           <table class="table">
             <thead>
                
                <th>Title</th>
                <th>Body</th>
                <th>Edit</th>
                <th>Delete</th>
             </thead>
             <tbody>
               <tr v-for="blog in blogs" :key="blog.url">
                 
                 <td>{{blog.title}}</td>
                 <td>{{blog.body}} </td>
                 <td>
                   <button @click="getOne(blog)" class="btn btn-sm btn-success"><i class="fa fa-pencil"></i></button>
                 </td>
                 <td>
                    <button @click="deleteOne(blog.url)" class="btn btn-sm btn-danger"><i class="fa fa-trash"></i></button>
                 </td>
               </tr>
             </tbody>
           </table> 
       </div>

  </div>
</template>

<script>
export default {
  name: 'Container',
  // props: {
  //   msg: String
  // },
  data(){
    return{
     blogs:null,
     url:'',
     title:'',
     body:''
    }
  },
  mounted(){
    this.getAll();

  },
  methods:{
    getAll(){
      axios.get(`http://localhost:8000/blogs`)
      .then((res)=>{
       this.blogs=res.data
       this.title='';
       this.url='';
       this.body='';
       this.url='';
       
      })
    },

    getOne(blog){
      this.url=blog.url;
      this.title=blog.title;
      this.body=blog.body;
    },
    deleteOne(url){
      axios.delete(url,{auth:{
        username:'narendra',
        password:'1234'
      }
       
      })
    },

    postBlog(){
      if(this.url==''){
             axios.post(`http://localhost:8000/blogs/`,
              {title:this.title,body:this.body},
              {auth:{username:'narendra',password:'1234'}}
               ).then(()=>{
              this.getAll();
              })
      }else{
         axios.put(this.url,
              {title:this.title,body:this.body},
              {auth:{username:'narendra',password:'1234'}}
               ).then(()=>{
              this.getAll();
              })
      }
    }

  }
}
</script>

<style scoped>

</style>
