<template>
	<span class="popover-wrap" ref="wrapper">
		<div :class="['popover', 'popover-'+look]" ref="popover" :style="{'display': realVisible, 'width': width+'px', 'top': top+'px', 'left': left+'px'}">
			<div class="popover-title" v-if="title != undefined">
				{{ title }}
			</div>
			<div class="popover-content" v-if="content != undefined">
				{{ content }}
			</div>
		</div>
		
		<slot></slot>
	</span>
</template>

<style lang="less">
	@import '~bootstrap/less/variables.less';
	@import '~bootstrap/less/mixins/alerts.less';

	.v-popover-variant(@bg, @border, @text) {
  		.alert-variant(@bg, @border, @text);
		.popover-title {
			font-weight: bold;
			background: @bg;
			border-bottom: 1px solid @border;
		}
		&:after, &:before {
			top: 100%;
			left: 50%;
			border: solid transparent;
			content: " ";
			height: 0;
			width: 0;
			position: absolute;
			pointer-events: none;
		}
		&:after {
			border-color: rgba(255, 255, 255, 0);
			border-top-color: @bg;
			border-width: 15px;
			margin-left: -15px;
		}
		&:before {
			border-color: rgba(187, 187, 187, 0);
			border-top-color: @border;
			border-width: 16px;
			margin-left: -16px;
		}
	}

	.btn-group span.popover-wrap {
		display: inline-block;
		&:not(:first-child) button {
			border-top-left-radius: 0;
			border-bottom-left-radius: 0;
		}
		&:not(:last-child) button {
			border-top-right-radius: 0;
			border-bottom-right-radius: 0;
		}
		.popover.popover-danger {
			.v-popover-variant(@alert-danger-bg; @alert-danger-border; @alert-danger-text);
		}
	}
	.btn-group .btn + .popover-wrap, .btn-group .popover-wrap + .btn {
		margin-left: -1px;
		position: relative;
		&:hover {
			z-index: 2;
		}
	}
	.btn-group .popover-wrap:first-child:hover {
		position: relative;
		z-index: 2;
	}
</style>

<script>
	export default {
		data: function() {
			return {
				left: 0,
				top: 0,
				visible: false
			}
		},
		props: {
			title: {
				require: false,
				default: undefined,
			},
			content: {
				require: false,
				default: undefined,
			},
			width: {
				default: 200,
				typee: Number,
				required: false
			},
			look: {
				required: false,
				default: 'default'
			}
		},
		computed: {
			realVisible: function() {
				return (this.visible) ? 'block' : 'none';
			}
		},
		methods: {
			show: function() {
				this.visible = true;
				this.$nextTick(function() {
					this.setPosition();
				});
			},
			setPosition: function() {
				this.left = ($(this.$refs.popover).width()/2-$(this.$refs.popover).next().width()-2) * -1;
				this.top = ($(this.$refs.popover).height() + 19) * -1;
			}
		},
		mounted: function() {
			var vm = this;

			this.$on('show', function() {
				vm.show();
			});

			this.$on('setposition', function() {
				vm.setPosition();
			});
		}
	}
</script>
