<template>
	<view class="add-model" v-if="visiableAddModel">
		<view class="emoji-images">
			<image 
				v-for="emoji in emojiUrl"
				:src="emoji.url" mode="" 
				:class="{active: form.emoji === emoji.alt}"
				class="emoji"
				@click="changeEmojiFn(emoji.alt)"></image> 
		</view>
		<view class="tags">
			<input
				type="text" 
				ref="tagInput"
				:focus="isTagFocus"
				v-model.trim="tagInputValue"
				maxlength="8"
				placeholder="mood tag"
				class="tag-input"
				@confirm="confirmTag"/>
				<view 
					v-for="(item, index) in form.tags"
					:key="item"
					class="tag">
					<text>{{ item }}</text>
					<text class="delete-btn" @click="deleteTag(index)">x</text>
				</view>
		</view>
		<view class="mood-input">
			<textarea 
				v-model="form.mood"
				placeholder="keep a record of your mood"
				class="textarea"/>
		</view>
		<view class="submit-btn">
			<text @click="addMoodFn">SUBMIT</text>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'AddModel',
		data() {
			return {
				visiableAddModel: false,
				tagInputValue: '',
				form: {
					emoji: 'peace',
					tags: [],
					mood: ''
				},
				isTagFocus: false,
				emojiUrl: [
					{
						url: require('../static/images/sad.png'),
						alt: 'sad'
					},{
						url: require('../static/images/happy.png'),
						alt: 'happy'
					},{
						url: require('../static/images/peace.png'),
						alt: 'peace'
					},{
						url: require('../static/images/sick.png'),
						alt: 'sick'
					},{
						url: require('../static/images/cool.png'),
						alt: 'cool'
					},{
						url: require('../static/images/anger.png'),
						alt: 'anger'
					},
				]
			}
		},
		methods: {
			show() {
				this.visiableAddModel = true
			},
			changeEmojiFn(emoji) {
				this.form.emoji = emoji
			},
			confirmTag(e) {
				const value = e.target.value
				if(!value) return
				this.form.tags.push(value) 
				this.$set(this, 'tagInputValue', '')
				this.isTagFocus = true
			},
			deleteTag(index) {
				this.form.tags.splice(index, 1)
			},
			addMoodFn() {
				const month = (new Date()).getMonth()+1
				const day = (new Date()).getDate()
				this.$emit('addMoodFn', Object.assign({},this.form, { month, day }))
				this.close()
			},
			close() {
				this.form = {
					emoji: 'peace',
					tags: [],
					mood: ''
				}
				this.tagInputValue = ''
				this.visiableAddModel = false
			}
		},
	}
</script>

<style scoped>
	.add-model {
		position: fixed;
		left: 50%;
		top:10%;
		transform: translateX(-50%);
		border-radius: 20px;
		width: 80%;
		min-height: 200px;
		background-color: #fff;
		box-shadow: 1px 1px 10px #C0C0C0;
		padding: 20px;
		z-index: 99;
		text-align: center;
	}
	/* emoji */
	.add-model .emoji-images {
		display: flex;
		justify-content: space-around;
	}
	.add-model .emoji-images .emoji {
		display: inline-block;
		width: 30px;
		height: 30px;
		opacity: 0.6;
		margin: 0 2%;
	}
	.add-model .emoji-images .emoji.active {
		opacity: 1;
	}
	/* tags */
	.add-model .tags {
		margin: 20px 0;
	}
	.add-model .tags .tag-input {
		border: 1px pink solid;
		border-radius: 10px;
		font-size:12px;
		height: 24px;
		line-height: 24px;
		margin-bottom: 10px;
	}
	.add-model .tags .tag {
		display: inline-block;
		font-size: 12px;
		border-radius:0 10px 10px 0;
		border:1px #fff591 solid;
		padding:0 4px;
		color: #999;
		margin:0 5px 0 0;
	}
	.add-model .tags .tag .delete-btn {
		color: pink;
		margin-left: 10px;
		padding-right: 5px;
	}
	/* mood */
	.add-model .mood-input .textarea {
		border: 1px #C0C0C0 solid;
		border-radius: 10px;
		margin: 10px 0;
		padding:8px;
		width: 100%;
		box-sizing: border-box;
		text-align: left;
	}
	/* submit */
	.add-model .submit-btn {
		width: 50%;
		margin: 0 auto;
		border: 1px #ffa931 solid;
		border-radius: 10px;
		box-shadow: 0 20px 50px 2px #ff8c1a inset; 
		color: #fff;
		font-weight: bold;
		line-height: 46px;
		height: 46px;
	}
</style>
