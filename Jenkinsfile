pipeline {
    agent any

    tools {
        sonarQubeScanner 'Sonarscanner'  // Matches the name in Jenkins > Global Tool Configuration
    }

    environment {
        // Define environment variables
        COMPOSER_HOME = '/tmp/composer'
        PATH = "${env.PATH}:/usr/local/bin"
        SONAR_PROJECT_KEY = 'dvwa-devsecops'
        SONAR_PROJECT_NAME = 'DVWA DevSecOps Project'
        SONAR_SOURCES = '.'
        SONAR_EXCLUSIONS = 'tests/**,vendor/**,external/**,docs/**,coverage/**'
        SONAR_HOST_URL = 'http://3.148.217.124:9000'
        SONAR_AUTH_TOKEN = credentials('SonarqubeToken') // Jenkins credential ID for
    }

    // tools {
    //     // Define SonarQube Scanner tool (needs to be configured in Jenkins)
    //     // Go to Manage Jenkins -> Global Tool Configuration -> SonarQube Scanner
    //     sonar 'sonar'
    // }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub repository
                git branch: 'future/jenkins', url: 'git@github.com:funmishade/DevSecOps_Project.git', credentialsId: 'sshagent'
            }
        }

//         stage('Install Dependencies & Tools') {
//             steps {
//                 echo 'Installing PHP dependencies and code analysis tools...'
//                 sh '''
//                     # Check if composer.json exists
//                     if [ -f composer.json ]; then
//                         # Install Composer if not available
//                         if ! command -v composer &> /dev/null; then
//                             echo "Installing Composer..."
//                             curl -sS https://getcomposer.org/installer | php
//                             sudo mv composer.phar /usr/local/bin/composer
//                             chmod +x /usr/local/bin/composer
//                         fi
                        
//                         # Install dependencies
//                         composer install --no-interaction --prefer-dist --optimize-autoloader
                        
//                         # Install development tools
//                         composer require --dev phpunit/phpunit
//                         composer require --dev squizlabs/php_codesniffer
//                         composer require --dev phpmd/phpmd
//                         composer require --dev sebastian/phpcpd
//                     else
//                         echo "No composer.json found. Installing tools globally..."
                        
//                         # Install PHPUnit globally
//                         if ! command -v phpunit &> /dev/null; then
//                             wget -O phpunit https://phar.phpunit.de/phpunit-9.phar
//                             chmod +x phpunit
//                             sudo mv phpunit /usr/local/bin/phpunit
//                         fi
                        
//                         # Install PHP CodeSniffer globally
//                         if ! command -v phpcs &> /dev/null; then
//                             echo "Installing PHP CodeSniffer..."
//                             wget https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar
//                             wget https://squizlabs.github.io/PHP_CodeSniffer/phpcbf.phar
//                             chmod +x phpcs.phar phpcbf.phar
//                             sudo mv phpcs.phar /usr/local/bin/phpcs
//                             sudo mv phpcbf.phar /usr/local/bin/phpcbf
//                         fi
                        
//                         # Install PHP Mess Detector
//                         if ! command -v phpmd &> /dev/null; then
//                             echo "Installing PHP Mess Detector..."
//                             wget https://phpmd.org/static/latest/phpmd.phar
//                             chmod +x phpmd.phar
//                             sudo mv phpmd.phar /usr/local/bin/phpmd
//                         fi
                        
//                         # Install PHP Copy/Paste Detector
//                         if ! command -v phpcpd &> /dev/null; then
//                             echo "Installing PHP Copy/Paste Detector..."
//                             wget https://phar.phpunit.de/phpcpd.phar
//                             chmod +x phpcpd.phar
//                             sudo mv phpcpd.phar /usr/local/bin/phpcpd
//                         fi
//                     fi
                    
//                     # Verify installations
//                     echo "Verifying tool installations:"
//                     php --version
//                     composer --version || echo "Composer not installed"
//                     phpcs --version || echo "PHP CodeSniffer not installed"
//                     phpmd --version || echo "PHP Mess Detector not installed"
//                     phpcpd --version || echo "PHP Copy/Paste Detector not installed"
//                 '''
//             }
//         }

//         stage('Unit Test') {
//             steps {
//                 echo 'Running PHPUnit Tests for DVWA...'
//                 sh '''
//                     # Create tests directory if it doesn't exist
//                     mkdir -p tests
                    
//                     # Create a phpunit.xml for DVWA structure
//                     cat > phpunit.xml << 'EOF'
// <?xml version="1.0" encoding="UTF-8"?>
// <phpunit bootstrap="tests/bootstrap.php"
//          colors="true"
//          convertErrorsToExceptions="true"
//          convertNoticesToExceptions="true"
//          convertWarningsToExceptions="true"
//          stopOnFailure="false">
//     <testsuites>
//         <testsuite name="DVWA Test Suite">
//             <directory>./tests</directory>
//         </testsuite>
//     </testsuites>
//     <coverage>
//         <include>
//             <directory suffix=".php">./</directory>
//         </include>
//         <exclude>
//             <directory>./tests</directory>
//             <directory>./docs</directory>
//             <directory>./external</directory>
//             <directory>./vendor</directory>
//         </exclude>
//     </coverage>
//     <logging>
//         <log type="coverage-clover" target="coverage.xml"/>
//         <log type="coverage-html" target="coverage"/>
//         <log type="junit" target="test-results.xml"/>
//     </logging>
// </phpunit>
// EOF

//                     # Create bootstrap file for tests
//                     cat > tests/bootstrap.php << 'EOF'
// <?php
// // Bootstrap file for DVWA tests
// $_SERVER['SERVER_NAME'] = 'localhost';
// $_SERVER['REQUEST_METHOD'] = 'GET';
// $_SERVER['REQUEST_URI'] = '/';

// // Include any necessary DVWA files
// if (file_exists('dvwa/includes/dvwaPage.inc.php')) {
//     require_once 'dvwa/includes/dvwaPage.inc.php';
// }
// EOF

//                     # Check if tests directory has any test files, if not create sample tests
//                     if ! ls tests/*Test.php 1> /dev/null 2>&1; then
//                         cat > tests/DVWABasicTest.php << 'EOF'
// <?php
// use PHPUnit\\Framework\\TestCase;

// class DVWABasicTest extends TestCase
// {
//     public function testIndexFileExists()
//     {
//         $this->assertFileExists('index.php');
//     }
    
//     public function testLoginFileExists()
//     {
//         $this->assertFileExists('login.php');
//     }
    
//     public function testSecurityFileExists()
//     {
//         $this->assertFileExists('security.php');
//     }
    
//     public function testConfigDirectoryExists()
//     {
//         $this->assertDirectoryExists('config');
//     }
    
//     public function testVulnerabilitiesDirectoryExists()
//     {
//         $this->assertDirectoryExists('vulnerabilities');
//     }
// }
// EOF

//                         cat > tests/DVWASecurityTest.php << 'EOF'
// <?php
// use PHPUnit\\Framework\\TestCase;

// class DVWASecurityTest extends TestCase
// {
//     public function testBasicSecurityChecks()
//     {
//         // Test that sensitive files don't exist in root
//         $this->assertFileNotExists('.env');
//         $this->assertFileNotExists('config.php.bak');
//     }
    
//     public function testDirectoryStructure()
//     {
//         $this->assertDirectoryExists('vulnerabilities');
//         $this->assertDirectoryExists('config');
//     }
// }
// EOF
//                     fi

//                     # Run PHPUnit tests
//                     if [ -f vendor/bin/phpunit ]; then
//                         ./vendor/bin/phpunit
//                     else
//                         phpunit
//                     fi
//                 '''
//             }
//             post {
//                 always {
//                     // Archive test results
//                     junit 'test-results.xml'
                    
//                     // Archive coverage reports
//                     publishHTML([
//                         allowMissing: true,
//                         alwaysLinkToLastBuild: true,
//                         keepAll: true,
//                         reportDir: 'coverage',
//                         reportFiles: 'index.html',
//                         reportName: 'Code Coverage Report'
//                     ])
//                 }
//             }
//         }

//         stage('PHP Code Quality Analysis') {
//             parallel {
//                 stage('PHP CodeSniffer') {
//                     steps {
//                         echo 'Running PHP CodeSniffer for coding standards...'
//                         sh '''
//                             # Create reports directory
//                             mkdir -p reports
                            
//                             # Run PHP CodeSniffer with different standards
//                             echo "Running PHP CodeSniffer with PSR12 standard..."
//                             if [ -f vendor/bin/phpcs ]; then
//                                 ./vendor/bin/phpcs --standard=PSR12 --extensions=php \
//                                     --report=checkstyle --report-file=reports/phpcs-checkstyle.xml \
//                                     --ignore=tests/,vendor/,external/,docs/ . || true
                                    
//                                 ./vendor/bin/phpcs --standard=PSR12 --extensions=php \
//                                     --report=full --report-file=reports/phpcs-full.txt \
//                                     --ignore=tests/,vendor/,external/,docs/ . || true
//                             else
//                                 phpcs --standard=PSR12 --extensions=php \
//                                     --report=checkstyle --report-file=reports/phpcs-checkstyle.xml \
//                                     --ignore=tests/,vendor/,external/,docs/ . || true
                                    
//                                 phpcs --standard=PSR12 --extensions=php \
//                                     --report=full --report-file=reports/phpcs-full.txt \
//                                     --ignore=tests/,vendor/,external/,docs/ . || true
//                             fi
                            
//                             echo "PHP CodeSniffer analysis completed."
//                         '''
//                     }
//                 }
                
//                 stage('PHP Mess Detector') {
//                     steps {
//                         echo 'Running PHP Mess Detector...'
//                         sh '''
//                             # Run PHP Mess Detector
//                             if [ -f vendor/bin/phpmd ]; then
//                                 ./vendor/bin/phpmd . xml cleancode,codesize,controversial,design,naming,unusedcode \
//                                     --reportfile reports/phpmd.xml \
//                                     --exclude tests/,vendor/,external/,docs/ || true
//                             else
//                                 phpmd . xml cleancode,codesize,controversial,design,naming,unusedcode \
//                                     --reportfile reports/phpmd.xml \
//                                     --exclude tests/,vendor/,external/,docs/ || true
//                             fi
                            
//                             echo "PHP Mess Detector analysis completed."
//                         '''
//                     }
//                 }
                
//                 stage('PHP Copy/Paste Detector') {
//                     steps {
//                         echo 'Running PHP Copy/Paste Detector...'
//                         sh '''
//                             # Run PHP Copy/Paste Detector
//                             if [ -f vendor/bin/phpcpd ]; then
//                                 ./vendor/bin/phpcpd --log-pmd reports/phpcpd.xml \
//                                     --exclude tests/ --exclude vendor/ --exclude external/ --exclude docs/ . || true
//                             else
//                                 phpcpd --log-pmd reports/phpcpd.xml \
//                                     --exclude tests/ --exclude vendor/ --exclude external/ --exclude docs/ . || true
//                             fi
                            
//                             echo "PHP Copy/Paste Detector analysis completed."
//                         '''
//                     }
//                 }
//             }
//             post {
//                 always {
//                     // Archive code quality reports
//                     archiveArtifacts artifacts: 'reports/*.xml,reports/*.txt', allowEmptyArchive: true
                    
//                     // Publish checkstyle results if available
//                     recordIssues enabledForFailure: true, tools: [checkStyle(pattern: 'reports/phpcs-checkstyle.xml')]
//                 }
//             }
//         }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Create sonar-project.properties file
                    writeFile file: 'sonar-project.properties', text: """
sonar.projectKey=${env.SONAR_PROJECT_KEY}
sonar.projectName=${env.SONAR_PROJECT_NAME}
sonar.projectVersion=1.0
sonar.sources=${env.SONAR_SOURCES}
sonar.exclusions=${env.SONAR_EXCLUSIONS}
sonar.language=php
sonar.sourceEncoding=UTF-8

# Code coverage
sonar.coverage.exclusions=tests/**,vendor/**,external/**,docs/**
sonar.php.coverage.reportPaths=coverage.xml

# External reports
sonar.php.phpunit.reportPaths=test-results.xml

# Additional PHP specific settings
sonar.php.file.suffixes=php,php3,php4,php5,phtml,inc
"""
                }
                
                // Run SonarQube analysis
                withSonarQubeEnv('sonar') {
                    sh '''
                        # Run SonarQube Scanner
                        sonar-scanner \
                            -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                            -Dsonar.projectName="${SONAR_PROJECT_NAME}" \
                            -Dsonar.sources=${SONAR_SOURCES} \
                            -Dsonar.exclusions="${SONAR_EXCLUSIONS}" \
                            -Dsonar.host.url=${SONAR_HOST_URL} \
                            -Dsonar.login=${SONAR_AUTH_TOKEN}
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Wait for SonarQube quality gate result
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running basic security checks...'
                sh '''
                    # Check for common security issues
                    echo "=== Security Scan Results ===" > reports/security-scan.txt
                    
                    # Check for potential SQL injection patterns
                    echo "Checking for potential SQL injection patterns:" >> reports/security-scan.txt
                    grep -rn --include="*.php" "\\$_GET\\|\\$_POST\\|\\$_REQUEST" . | head -20 >> reports/security-scan.txt || true
                    
                    # Check for eval() usage
                    echo "Checking for eval() usage:" >> reports/security-scan.txt
                    grep -rn --include="*.php" "eval(" . >> reports/security-scan.txt || true
                    
                    # Check for file inclusion vulnerabilities
                    echo "Checking for file inclusion patterns:" >> reports/security-scan.txt
                    grep -rn --include="*.php" "include\\|require" . | head -10 >> reports/security-scan.txt || true
                    
                    # Check for hardcoded credentials
                    echo "Checking for potential hardcoded credentials:" >> reports/security-scan.txt
                    grep -rn --include="*.php" -i "password\\|passwd\\|pwd" . | head -10 >> reports/security-scan.txt || true
                    
                    echo "Security scan completed. Check reports/security-scan.txt for details."
                '''
            }
            post {
                always {
                    archiveArtifacts artifacts: 'reports/security-scan.txt', allowEmptyArchive: true
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed'
            
            // Archive all reports
            archiveArtifacts artifacts: 'reports/**', allowEmptyArchive: true
            
            // Clean up workspace
            cleanWs()
        }
        success {
            echo 'All tests and quality checks passed!'
            
            // Send success notification (configure as needed)
            // emailext subject: "Pipeline Success: ${env.JOB_NAME} - ${env.BUILD_NUMBER}",
            //          body: "The pipeline completed successfully.",
            //          to: "your-email@example.com"
        }
        failure {
            echo 'Pipeline failed!'
            
            // Send failure notification (configure as needed)
            // emailext subject: "Pipeline Failed: ${env.JOB_NAME} - ${env.BUILD_NUMBER}",
            //          body: "The pipeline failed. Please check the logs.",
            //          to: "your-email@example.com"
        }
        unstable {
            echo 'Pipeline is unstable (some quality gates failed)'
        }
    }
}