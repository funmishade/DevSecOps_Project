/*
pipeline {
    agent any
    
    environment {
        // Define any environment variables if needed
        GIT_REPO = 'git@github.com:hashicorp/terraform.git'
        GIT_BRANCH = 'future/jenkins'
        SSH_CREDENTIAL_ID = 'sshagent'
    }
    
    stages {
        stage('Git Clone') {
            steps {
                // Clean workspace before cloning
                cleanWs()
                
                // Clone the repository using SSH
                sshagent(credentials: ["${SSH_CREDENTIAL_ID}"]) {
                    sh '''
                        git clone -b ${GIT_BRANCH} ${GIT_REPO} .
                        echo "Successfully cloned ${GIT_REPO} on branch ${GIT_BRANCH}"
                        git log --oneline -5
                    '''
                }
            }
        }
        
        stage('Build') {
            steps {
                echo 'Add your build steps here'
                // Example build commands:
                // sh 'make build'
                // sh 'go build .'
                // sh 'terraform init'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Add your test steps here'
                // Example test commands:
                // sh 'make test'
                // sh 'go test ./...'
                // sh 'terraform validate'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Add your deployment steps here'
                // Example deployment commands:
                // sh 'terraform plan'
                // sh 'terraform apply -auto-approve'
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed'
            // Clean up workspace
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
*/

// pipeline {
//     agent any

//     stages {
//         stage('Checkout') {
//             steps {
//                 // Checkout code from GitHub repository
//                 git branch: 'future/jenkins', url: 'git@github.com:funmishade/DevSecOps_Project.git', credentialsId: 'sshagent'
//             }
//         }

//         stage('Unit Test') {
//             steps {
//                 echo 'Running Unit Tests...'
//                 // Add your command to run unit tests
//                 sh './run-tests.sh'  // Example: Running a test script
//             }
//         }
//     }
// }


pipeline {
    agent any



    environment {
        // Define environment variables
        COMPOSER_HOME = '/tmp/composer'
        PATH = "${env.PATH}:/usr/local/bin"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub repository
                git branch: 'future/jenkins', url: 'git@github.com:funmishade/DevSecOps_Project.git', credentialsId: 'sshagent'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing PHP dependencies...'
                sh '''
                    # Check if composer.json exists
                    if [ -f composer.json ]; then
                        # Install Composer if not available
                        if ! command -v composer &> /dev/null; then
                            curl -sS https://getcomposer.org/installer | php
                            sudo mv composer.phar /usr/local/bin/composer
                            chmod +x /usr/local/bin/composer
                        fi
                        
                        # Install dependencies
                        composer install --no-interaction --prefer-dist --optimize-autoloader
                        
                        # Install PHPUnit if not in composer.json
                        if ! composer show phpunit/phpunit &> /dev/null; then
                            composer require --dev phpunit/phpunit
                        fi
                    else
                        echo "No composer.json found. Installing PHPUnit globally..."
                        # Install PHPUnit globally via wget
                        wget -O phpunit https://phar.phpunit.de/phpunit-9.phar
                        chmod +x phpunit
                        sudo mv phpunit /usr/local/bin/phpunit
                    fi
                '''
            }
        }

        stage('Unit Test') {
            steps {
                echo 'Running PHPUnit Tests for DVWA...'
                sh '''
                    # Create a phpunit.xml for DVWA structure
                    cat > phpunit.xml << 'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<phpunit bootstrap="tests/bootstrap.php"
         colors="true"
         convertErrorsToExceptions="true"
         convertNoticesToExceptions="true"
         convertWarningsToExceptions="true"
         stopOnFailure="false">
    <testsuites>
        <testsuite name="DVWA Test Suite">
            <directory>./tests</directory>
        </testsuite>
    </testsuites>
    <filter>
        <whitelist>
            <directory suffix=".php">./</directory>
            <exclude>
                <directory>./tests</directory>
                <directory>./docs</directory>
                <directory>./external</directory>
            </exclude>
        </whitelist>
    </filter>
</phpunit>
EOF

                    # Create bootstrap file for tests
                    cat > tests/bootstrap.php << 'EOF'
<?php
// Bootstrap file for DVWA tests
$_SERVER['SERVER_NAME'] = 'localhost';
$_SERVER['REQUEST_METHOD'] = 'GET';
$_SERVER['REQUEST_URI'] = '/';

// Include any necessary DVWA files
if (file_exists('dvwa/includes/dvwaPage.inc.php')) {
    require_once 'dvwa/includes/dvwaPage.inc.php';
}
EOF

                    # Check if tests directory has any test files, if not create sample tests
                    if [ ! -f tests/*Test.php ] 2>/dev/null; then
                        cat > tests/DVWABasicTest.php << 'EOF'
<?php
use PHPUnit\\Framework\\TestCase;

class DVWABasicTest extends TestCase
{
    public function testIndexFileExists()
    {
        $this->assertFileExists('index.php');
    }
    
    public function testLoginFileExists()
    {
        $this->assertFileExists('login.php');
    }
    
    public function testSecurityFileExists()
    {
        $this->assertFileExists('security.php');
    }
    
    public function testConfigDirectoryExists()
    {
        $this->assertDirectoryExists('config');
    }
    
    public function testDatabaseDirectoryExists()
    {
        $this->assertDirectoryExists('database');
    }
    
    public function testVulnerabilitiesDirectoryExists()
    {
        $this->assertDirectoryExists('vulnerabilities');
    }
}
EOF

                        cat > tests/DVWASecurityTest.php << 'EOF'
<?php
use PHPUnit\\Framework\\TestCase;

class DVWASecurityTest extends TestCase
{
    public function testPhpIniFileExists()
    {
        $this->assertFileExists('php.ini');
    }
    
    public function testSecurityTxtExists()
    {
        $this->assertFileExists('security.txt');
    }
    
    public function testRobotsTxtExists()
    {
        $this->assertFileExists('robots.txt');
    }
    
    public function testReadmeExists()
    {
        $this->assertFileExists('README.md');
    }
    
    public function testChangelogExists()
    {
        $this->assertFileExists('CHANGELOG.md');
    }
}
EOF
                    fi

                    # Run PHPUnit tests
                    if [ -f vendor/bin/phpunit ]; then
                        ./vendor/bin/phpunit --log-junit test-results.xml --coverage-html coverage
                    else
                        phpunit --log-junit test-results.xml --coverage-html coverage
                    fi
                '''
            }
            post {
                always {
                    // Archive test results
                    junit 'test-results.xml'
                    
                    // Archive coverage reports
                    publishHTML([
                        allowMissing: true,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'coverage',
                        reportFiles: 'index.html',
                        reportName: 'Code Coverage Report'
                    ])
                }
            }
        }

        stage('Code Quality Check') {
            steps {
                echo 'Running PHP Code Quality Checks for DVWA...'
                sh '''
                    # Install PHP CodeSniffer if not available
                    if ! command -v phpcs &> /dev/null; then
                        if [ -f composer.json ]; then
                            composer require --dev squizlabs/php_codesniffer
                        else
                            wget https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar
                            chmod +x phpcs.phar
                            sudo mv phpcs.phar /usr/local/bin/phpcs
                        fi
                    fi

                    # Run PHP CodeSniffer on main PHP files
                    if [ -f vendor/bin/phpcs ]; then
                        ./vendor/bin/phpcs --standard=PSR2 --extensions=php *.php vulnerabilities/ || true
                    else
                        phpcs --standard=PSR2 --extensions=php *.php vulnerabilities/ || true
                    fi
                    
                    # Run PHP syntax check on all PHP files
                    echo "Running PHP syntax check..."
                    find . -name "*.php" -not -path "./tests/*" -not -path "./external/*" | xargs -I {} php -l {} || true
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed'
            // Clean up workspace
            cleanWs()
        }
        success {
            echo 'All tests passed!'
        }
        failure {
            echo 'Tests failed!'
        }
    }
}