/* eslint-disable */
<template>
    <div class="container">
        <h1>Vue.js + Github</h1>
        <p class="lead">
            Página que lista issues de um repositório do Github, usando Vue.js.
        </p>

        <div class="row">
            <div class="col">
                <div class="form-group">
                    <input v-model="username"
                    type="text" class="form-control" placeholder="github username">
                </div>
            </div>

            <div class="col">
                <div class="form-group">
                    <input v-model="repository"
                    type="text" class="form-control" placeholder="github repositório">
                </div>
            </div>

            <div class="col-3">
                <div class="form-group">
                    <button @click.prevent.stop="getIssues()" class="btn btn-success">GO</button>
                    <button @click.prevent.stop="reset()" class="btn btn-danger">LIMPAR</button>
                </div>
            </div>
        </div>
        <br><hr><br>
        <template v-if="selectedIsseu.id">
        	<h2>{{selectedIsseu.title}}</h2>
        	<p>{{selectedIsseu.body}}</p>
            <button @click.prevent.stop="clearIsseu()" class="btn btn-info">Voltar</button>
        </template>
        <table  v-if=" !selectedIsseu.id" class="table table-sm table-bordered">
            <thead>
            <tr>
                <th width="100">Número</th>
                <th>Título</th>
            </tr>
            </thead>

            <tbody>
            	<tr v-if="loader.getIssues">
            		<td class="text-center" colspan="2"><img src="static\Eclipse.svg"></td>
            	</tr>
            	<tr v-if="!!issues.length && !loader.getIssues"
            	v-for="issue in issues"
            	:key="issue.number">
            		<td><a @click.prevent.stop="getIssue(issue.number)"
            			   href="">{{issue.number}}</a></td>
            		<td>{{issue.title}}</td>
            	</tr>
            <tr v-if="!!!issues.length && !loader.getIssues">
                <td class="text-center" colspan="2">Nenhuma issues encontrada!</td>
            </tr>
            </tbody>
        </table>
    </div>
</template>
<script>
    import axios from 'axios';
	export default {
		name: 'GitHubIssues',

		data() {
			return {
                username:'',
                repository:'',
                issues:[],
                selectedIsseu:{},
                loader:{
                	getIssues:false,
                	getIsseu: false,
                }
			};
		},
		methods:{
				reset(){
					this.username = '';
					this.repository = '';
				},

				getIssues(){
					if (this.username && this.repository){
					this.loader.getIssues = true;
					const url = `https://api.github.com/repos/${this.username}/${this.repository}/issues`;
		                axios.get(url).then((response) => {
		                	this.issues = response.data;
		                }).finally(()=>{
		                	this.loader.getIssues = false;
		                });
	                }
				},
				getIssue(issueId){
						if (this.username && this.repository){
						this.loader.getIssue = true;
						const url = `https://api.github.com/repos/${this.username}/${this.repository}/issues/${issueId}`;
			                axios.get(url).then((response) => {
			                	this.selectedIsseu = response.data;
			                }).finally(()=>{
			                	this.loader.getIssue = false;
			                });
		                }
				},
				clearIsseu(){
					this.selectedIsseu={};
				}
		}
	};
</script>


