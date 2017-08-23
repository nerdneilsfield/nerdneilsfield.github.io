<template>
	<div class="issueslist typo">
		<span v-if="showmessage">
		<h2> {{ message }} </h2>
		</span>
			<!-- <button @click="pagelast" class="pure-button"><i class="fa fa-chevron-circle-left" aria-hidden="true"></i>last</button>
			<input v-model="showpage" @keyup.enter="getissues(page)"> -->
			<!-- <button @click="getissues(page)">Load</button> -->
			<!-- <button @click="pagenext" class="pure-button"><i class="fa fa-chevron-circle-right" aria-hidden="true"></i>next</button> -->
		<hr />
		<div v-if="show">
			<div v-for="issue in issues">
				<h2 id="title">
						<span v-html="rendermarkdown(issue.title)"></span>
				</h2>
				<p>
				<span id="time" class="pure-u-5-8">
					<i class="fa fa-clock-o" aria-hidden="true"></i>{{ issue.updated_at.slice(0,10) }}
				</span>
				<span id="author" class="pure-u-1-3"><img class="avator" :src="issue.user.avatar_url">
					<b> {{ issue.user.login }} </b>
				</span>
				</p>
				<span v-html="rendermarkdown(issue.body)"></span>
				<hr />
			</div>
			<button @click="pagelast" class="pure-button pure-u-2-5"><i class="fa fa-chevron-circle-left" aria-hidden="true"></i>last</button>
			<input v-model="showpage" @keyup.enter="getissues(page)" class="hama" >
			<!-- <button @click="getissues(page)">Load</button> -->
			<button @click="pagenext" class="pure-button pure-u-2-5"><i class="fa fa-chevron-circle-right" aria-hidden="true"></i>next</button>
			<hr />
		</div>
	</div>
</template>
<script type="text/javascript">
var marked = require('marked');
var axios = require('axios');
var hljs = require('highlight.js'); // https://highlightjs.org/
// Actual default values
var md = require('markdown-it')({
	highlight: function (str, lang) {
		if (lang && hljs.getLanguage(lang)) {
			try {
				return '<pre class="hljs"><code>' +
					hljs.highlight(lang, str, true).value +
					'</code></pre>';
			} catch (__) { }
		}
		return '<pre class="hljs"><code>' + md.utils.escapeHtml(str) + '</code></pre>';
	}
});
var markdown = require("markdown").markdown;
marked.setOptions({
	highlight: function (code) {
		return require('highlight.js').highlightAuto(code).value;
	}
});
export default {
	name: 'issueslist',
	data() {
		return {
			issues: [],
			message: 'Loading!',
			show: false,
			page: 1,
			cutts: 0,
			showmessage: false
		}
	},
	computed:{
		showpage: {
			get: function() {
				return (this.page - 1) * 5 + this.cutts + 1;
			},
			set: function(newpage) {
				this.page = newpage % 5 + 1;
				this.cutts = newpage - (this.page - 1) * 5 - 1;
			}
		}
	},
	created : function() {
		this.getissues();
	},
	methods: {
		getissues: function () {
			var vm = this
			axios.get('https://api.github.com/repos/nerdneilsfield/302/issues?state=closed&page=' + this.page.toString())
				.then(function (response) {
					vm.message = 'Load successful!';
					var capacity = response.data.length;
					var cut = vm.cutts;
					if (cut * 5 < capacity) {
						if (cut * 5 + 5 < capacity) {
							vm.issues = response.data.slice(5 * cut, cut * 5 + 5);
						} else {
							vm.issues = response.data.slice(5 * cut);
						}
					} else {
						vm.issues = response.data.slice(capacity - 5);
					}
					vm.show = true;
					vm.showmessage = false;
					
				})
				.catch(function (error) {
					vm.showmessage = true;
					vm.message = "Error" + error.toString();
				});
		},
		rendermarkdown: function (text) {
			if (!text) {
				return "<b>No Text<b>"
			} else {
				// return marked(text);
				// return markdown.toHTML(text);
				return md.render(text);
			}
		},
		pagelast: function () {
			if (this.cutts > 0) {
				this.cutts = this.cutts - 1;
			} else {
				this.page = this.page > 1 ? this.page - 1 : this.page;
				this.cutts = 0;
			}
			this.getissues();
		},
		pagenext: function () {
			if (this.cutts < 5) {
				this.cutts = this.cutts + 1;
			} else {
				this.page = this.page + 1;
				this.cutts = 0;
			}

			this.getissues();
		},
	}
}
</script>
<style type="text/css">
.avator {
	max-height: 16px;
	max-width: 16px;
}


.hama {
	align-content: center;
	text-align: center;
	float: center;
	width: 15%;
}


#time {
	text-align: left;
}

#author {
	text-align: right;
}

 #title {
	text-shadow: silver;
	text-align: center;
}

.shut {
	width: 15% ;
}

/*	hr {
		border-style:  dashed;
	}*/
</style>
