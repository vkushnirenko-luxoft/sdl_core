pipeline {
    agent { label 'atf_slave' }
    environment { 
        THIRD_PARTY_INSTALL_PREFIX = "${WORKSPACE}/build/src/3rdparty/LINUX"
        THIRD_PARTY_INSTALL_PREFIX_ARCH="${THIRD_PARTY_INSTALL_PREFIX}/x86"
        LD_LIBRARY_PATH="$THIRD_PARTY_INSTALL_PREFIX_ARCH/lib:$THIRD_PARTY_INSTALL_PREFIX/lib"
    }
    stages {
        stage('Preparation') {
            steps {
                sh 'ulimit -c unlimited'
                git branch: 'develop', url: 'file:////data/github/sdl_core'
                sh 'ls -la'
            }
        }
        stage('cmake') {
            steps {
                cmakeBuild buildDir: 'build', cmakeArgs: '-DBUILD_TESTS=O', installation: 'InSearchPath', sourceDir: '.'
            }
        }
        stage('build') {
            steps {
                dir('build') {
                    sh 'make install'
                    sh 'tar -zcf OpenSDL.tar.gz bin'
                    archive 'build/OpenSDL.tar.gz'
                }
            }
        }
    }
}