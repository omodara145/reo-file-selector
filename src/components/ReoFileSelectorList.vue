<template>
  <ul class="selector-list">
		<!-- Render Folders -->
    <li
      v-for="folder in folders"
      :key="folder.id"
      class="selector-list__item"
       @click="navigate(folder)">
      <div>
        <span class="image">
					<img :src="getImage('folder.svg')" alt="F" />
				</span>
        <p>{{ folder.name }}</p>
      </div>
			<reo-icon :icon="'chevron-right'" :hover="false"></reo-icon>
    </li>
		<!-- Render Files -->
		<li
				v-for="file in files"
				:key="file.id"
				class="selector-list__item"
				@click="action(file)">
			<div>
        <span class="image">
          <img v-if="file.mimeType.includes('image')" :src="getFileImage(file.url)" alt="D" class="full-image" />
          <img v-else :src="getFileImage(file.url)" alt="D" />
        </span>
				<p>{{ file.name }}</p>
			</div>
			<span
					v-if="selectedFilesId.includes(file.id)">
        <img :src="getImage('selected.svg')" alt=">" />
      </span>
		</li>
  </ul>
</template>

<script>

import ReoIcon from '@/components/ReoIcon'

export default {
	name: 'ReoFileSelectorList',
	components: { ReoIcon },
	props: {
    selections: {
      type: Array,
      default () {
        return []
      }
    },
		folders: {
			type: Array,
			default () {
				return []
			}
		},
		files: {
			type: Array,
			default () {
				return []
			}
		}
	},
	data () {
		return {}
	},
	computed: {
	  selectedFilesId () {
      return this.selections.map(selection => selection.id)
    },
		updateSelectedFiles: {
			get () {
				return this.selections
			},
			set (value) {
				this.$emit('update:selections', value)
			}
		}
	},
	methods: {
		navigate (data) {
			this.$emit('navigate', data)
		},
		action (file) {
			/**
			 * Clicking on a File, which toggles add or removal to selection
			 * Check if a file exist in the selection, if yes, remove it, if not, add to selection.
			 */
		  this.selectedFilesId.includes(file.id) ? this.removeFile(file.id) : this.addFile(file)
		},
    addFile (file) {
      this.updateSelectedFiles.push(file)
    },
    removeFile (fileId) {
      this.updateSelectedFiles = this.updateSelectedFiles.filter(file => file.id !== fileId)
    }
	}
}
</script>

<style lang="scss">
.selector-list {
  list-style-type: none;
  padding: 0;
  margin: 0;
  height: 256px;
  overflow-y: scroll;

  &__item {
    padding: 8px;
    border-radius: 8px;
    cursor: pointer;
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: transparent;
    transition: background 0.25s ease-in;

    > div {
      display: flex;
      align-items: center;

      p {
        font-size: 18px;
        color: #262626;
      }

      .image {
        height: 28px;
        width: 28px;
        display: flex;
        justify-content: center;
        align-items: center;
        background: rgba(0, 0, 0, 0.04);
        border-radius: 4px;
        margin-right: 12px;

        .full-image {
          border-radius: 4px;
          height: 100%;
          width: 100%;
          object-fit: cover;
        }
      }
    }

    span {
      display: flex;
      align-items: center;
    }

    &:hover {
      background: rgba(0, 0, 0, 0.04);
      transition: background 0.25s ease-in;
    }

    &:active {
      background: rgba(0, 0, 0, 0.08);
      transition: background 0.25s ease-in;
    }
  }
}
</style>
