pipeline {
  agent {
    node {
      label 'android-x86 && repo'
    }
    
  }
  stages {
    stage('Init') {
      steps {
        sh 'repo init -u https://gogs.thedrhax.pw/t100ta-android/manifest.git -b marshmallow-x86 --depth=1'
      }
    }
    stage('Sync') {
      steps {
        sh 'repo sync --no-tags --no-clone-bundle --force-sync -j4'
      }
    }
    stage('Build') {
      steps {
        sh '''. build/envsetup.sh
lunch 8
make iso_img -j4'''
      }
    }
    stage('Archive') {
      steps {
        archiveArtifacts 'out/target/product/x86_64/android_x86_64.iso'
      }
    }
  }
  environment {
    USE_CCACHE = '1'
    CCACHE_DIR = '.ccache'
    PATH = '/usr/lib/jvm/java-7-openjdk-amd64/bin:$PATH'
  }
}