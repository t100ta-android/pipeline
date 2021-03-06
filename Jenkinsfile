pipeline {
  agent {
    node {
      label 'android-x86'
      customWorkspace 'marshmallow-x86'
    }
  }
  stages {
    stage('Init') {
      steps {
        sh 'repo init -u https://github.com/t100ta-android/manifest.git -b marshmallow-x86 --depth=10'
      }
    }
    stage('Sync') {
      steps {
        sh 'repo sync --no-tags --no-clone-bundle --force-sync -j4'
        sh 'repo manifest -r -o static-manifest.xml'
      }
    }
    stage('Build') {
      steps {
        sh 'bash -c ". build/envsetup.sh && lunch 8 && make iso_img -j4"'
      }
    }
    stage('Archive') {
      steps {
        archiveArtifacts 'out/target/product/x86_64/android_x86_64.iso'
        archiveArtifacts 'static-manifest.xml'
      }
    }
  }
  environment {
    USE_CCACHE = '1'
    CCACHE_DIR = "$SLAVE_HOME/.ccache"
    PATH = "/usr/lib/jvm/java-7-openjdk-amd64/bin:$PATH"
    TARGET_KERNEL_CONFIG = 't100ta-x86_64_defconfig'
  }
}
