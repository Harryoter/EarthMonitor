<template>
  <div class="potree_container" style="width: 100%; height: 100%">
    <div id="potree_render_area"></div>
    </div>
</template>

<script>
//import $ from ''
export default {
  name: 'HelloPotree',
  data () {
    return {
      msg: 'Welcome to Potree Vue.js App'
    }
  },
  mounted(){
	var canvas = document.getElementById("potree_render_area")
    window.viewer = new Potree.Viewer(canvas)
		viewer.setEDLEnabled(true)
		viewer.setFOV(60)
		viewer.setPointBudget(1*1000*1000)
		viewer.loadSettingsFromURL()
		
		viewer.setDescription("Loading Octree of LAS files")
		Potree.measureTimings = true;
		//viewer.loadGUI(() => {});
		
		// Sigeom
		Potree.loadPointCloud("static/pointclouds/lion_takanawa_las/cloud.js", "lion", function(e){
			viewer.scene.addPointCloud(e.pointcloud);
			
			let material = e.pointcloud.material;
			material.size = 1;
			material.pointSizeType = Potree.PointSizeType.ADAPTIVE;
			
			e.pointcloud.position.x += 3;
			e.pointcloud.position.y -= 3;
			e.pointcloud.position.z += 4;
			
			viewer.fitToScreen();
		});
		const animation = new Potree.CameraAnimation(viewer);


		const positions = [
			[590291.6145250637, 231565.3152460147, 888.181158774433],
			[590094.2454560432, 231235.32163877538, 870.7535717968211],
			[589675.8154371583, 231058.22066649256, 905.3068746322883],
			[589328.6700949036, 231385.37585641106, 813.9565903445384],
		];

		const targets = [
			[589859.3465488373, 231456.18943956672, 758.2733646218901],
			[589846.4463098792, 231431.89813285187, 755.9090168440739],
			[589824.0843049305, 231444.72309070674, 760.3459659610106],
			[589799.7263767472, 231473.79043369304, 758.8332698380435],
		];

		for(let i = 0; i < positions.length; i++){
			const cp = animation.createControlPoint();

			cp.position.set(...positions[i]);
			cp.target.set(...targets[i]);
		}

		viewer.scene.addCameraAnimation(animation);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
